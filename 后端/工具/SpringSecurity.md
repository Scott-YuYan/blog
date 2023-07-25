Spring家族一员，充分利用了Spring Ioc,DI和AOP功能，完全支持自定义，兼容性更好。最重要的两个核心功能是：认证和授权<br/>

在项目中引入SpringSecurity:
```
 <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-security</artifactId>
 </dependency>
```
启动项目访问端口后，此时SpringSecurity会进入默认的登陆页面，用户名为user，密码为启动日志中的："sing generated security password: xxxx"，
默认的登陆逻辑下载UserDetailsService中，后续也需要实现该接口重写loadUserByUsername(String username)方法。
密码的比较在PasswordEncoder中，后续需要在config中使用BCryptPasswordEncoder替换：
```
 * Encode the raw password. Generally, a good encoding algorithm applies a SHA-1 or
 * greater hash combined with an 8-byte or greater randomly generated salt.
```
在它的实现类中，推荐使用BCryptPasswordEncoder:
```
 PasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
 String encode = passwordEncoder.encode("123");
 System.out.println(encode);
 boolean matches = passwordEncoder.matches("123", encode);
 System.out.println(matches);
```

<img width="1001" alt="image" src="https://github.com/Scott-YuYan/blog/assets/51253421/d514924f-f2b0-4e17-885b-2e14855127b9">

SpringSecurity流程：
登陆：
   1.自定义登陆接口：
       调用ProviderManager的方法进行认证，如果认证通过则生成JWT，并且将用户信息存入Redis中，
   2.自定义UserDetailService，在这个实现方法中查询用户信息    
检验：
   1.定义JWT认证过滤器，获取token，解析token获取token中的userId，并且从redis中获取用户信息，并存入SecurityContextHolder。

SpringSecurity从入门到精通【三更草堂笔记】 ： https://blog.csdn.net/qq_45847507/article/details/126681110
完成项目基本配置后，首先实现UserDetailService接口，重写loadUserByUsername方法：
```
        //1.数据库查询用户
        //根据用户名查询用户信息
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUserName,username);
        User user = userMapper.selectOne(wrapper);
        //如果查询不到数据就通过抛出异常来给出提示
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或密码错误");
        }
        //TODO 根据用户查询权限信息 添加到LoginUser中

        //封装成UserDetails对象返回
        return new LoginUser(user);
```
SpringSecurity在当中，通过loadUserByUsername差到用户之后，会调用默认的passwordEncoder.matcher()方法校验传入密码，但是由于默认的加密需要在密码签加入{encoder}、
或者不加密使用{noop}，不方便使用，所以进一步替换Security的默认PasswordEncoder;
```
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
```
并且使用AuthenticationManager.authenticate()方法来进行用户认证：
```
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
```
接下来继续在SecurityConfig中增加放行登陆页的配置：
```
    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception {
        // 自定义登陆页面
        httpSecurity.formLogin().loginProcessingUrl("/login");
        //授权
        httpSecurity.authorizeRequests()
                // 登陆页面放行
                .antMatchers("/login").permitAll()
                //所有的请求都必须认证后才能访问,必须登陆
                .anyRequest().authenticated();
        //关闭csrf防护
        httpSecurity.csrf().disable();

    }
```
写登陆方法，LoginServiceImpl具体实现类：
```
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.getUserName(),user.getPassword());
        // Authentication验证的时候会调用UserDetailService中的loadUserByUsername方法，进一步调用BCryptPasswordEncoder解析密码
        Authentication authenticate = authenticationManager.authenticate(authenticationToken);
        if(Objects.isNull(authenticate)){
            throw new RuntimeException("用户名或密码错误");
        }
        //使用userid生成token
        LoginUser loginUser = (LoginUser) authenticate.getPrincipal();
        String userId = loginUser.getUser().getId().toString();
        String jwt = JwtUtil.createJWT(userId);
        //authenticate存入redis
        redisCache.setCacheObject("login:"+userId,loginUser);
        //把token响应给前端
        HashMap<String,String> map = new HashMap<>();
        map.put("token",jwt);
        return new ResponseResult(200,"登陆成功",map);
```
接下来进行自定义认证过滤器，获取请求头中的token，并解析获取到LoginUser对象，放到SecurityContextHolder中：
```
@Component
public class JwtAuthenticationTokenFilter extends OncePerRequestFilter {

    @Autowired
    private RedisCache redisCache;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, 
                                    FilterChain filterChain) throws ServletException, IOException {
        //获取token
        String token = request.getHeader("token");
        if (!StringUtils.hasText(token)) {
            //放行
            filterChain.doFilter(request, response);
            return;
        }
        //解析token
        String userid;
        try {
            Claims claims = JwtUtil.parseJWT(token);
            userid = claims.getSubject();
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException("token非法");
        }
        //从redis中获取用户信息
        String redisKey = "login:" + userid;
        LoginUser loginUser = redisCache.getCacheObject(redisKey);
        if(Objects.isNull(loginUser)){
            throw new RuntimeException("用户未登录");
        }
        //存入SecurityContextHolder
        //TODO 获取权限信息封装到Authentication中
        UsernamePasswordAuthenticationToken authenticationToken =
                new UsernamePasswordAuthenticationToken(loginUser,null,null);
        SecurityContextHolder.getContext().setAuthentication(authenticationToken);
        //放行
        filterChain.doFilter(request, response);
    }
}
```
完成自定义过滤器的编写后，需要将自定义过滤器添加到配置中：
```
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/user/login").anonymous()
                // 除上面外的所有请求全部需要鉴权认证
                .anyRequest().authenticated();

        //把token校验过滤器添加到过滤器链中
        http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);
    }
```
之后系统接口被调用时，都会检查请求头中是否带token，如果带则进一步解析token获取userId，并根据userId查询redis获取用户信息，并加入到SpringSecurityContext中。<br/>
登出接口则删除redis中缓存。


在SpringSecurity中，会使用默认的FilterSecurityInterceptor来进行权限校验。在FilterSecurityInterceptor中会从SecurityContextHolder获取其中的Authentication，
然后获取其中的权限信息。当前用户是否拥有访问当前资源所需的权限。所以我们在项目中只需要把当前登录用户的权限信息也存入Authentication。<br/>
启动类开启授权相关配置：
```
@EnableGlobalMethodSecurity(prePostEnabled = true)
```
在指定接口增加注解：
```
@RestController
public class HelloController {

    @RequestMapping("/hello")
    @PreAuthorize("hasAuthority('test')")
    public String hello(){
        return "hello";
    }
}

```
配置好权限数据库后，然后在UserDetailService的loadUserByUsername方法中获取并加到LoginUser中：
```
        List<String> permissionKeyList =  menuMapper.selectPermsByUserId(user.getId());
        return new LoginUser(user,permissionKeyList);
```


自定义失败处理：
如果是认证过程中出现的异常会被封装成AuthenticationException然后调用AuthenticationEntryPoint对象的方法去进行异常处理。

如果是授权过程中出现的异常会被封装成AccessDeniedException然后调用AccessDeniedHandler对象的方法去进行异常处理。

所以如果我们需要自定义异常处理，我们只需要自定义AuthenticationEntryPoint和AccessDeniedHandler然后配置给SpringSecurity即可。

```
@Component
public class AccessDeniedHandlerImpl implements AccessDeniedHandler {
    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException accessDeniedException) throws IOException, ServletException {
        ResponseResult result = new ResponseResult(HttpStatus.FORBIDDEN.value(), "权限不足");
        String json = JSON.toJSONString(result);
        WebUtils.renderString(response,json);

    }
}

/**
 * @Author 三更  B站： https://space.bilibili.com/663528522
 */
@Component
public class AuthenticationEntryPointImpl implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        ResponseResult result = new ResponseResult(HttpStatus.UNAUTHORIZED.value(), "认证失败请重新登录");
        String json = JSON.toJSONString(result);
        WebUtils.renderString(response,json);
    }
}

接下来在config中配置：
        http.exceptionHandling().authenticationEntryPoint(authenticationEntryPoint).
                accessDeniedHandler(accessDeniedHandler);
```

自定义认证成功处理器：
```
@Component
public class SuccessHandlerImpl implements AuthenticationSuccessHandler {

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
        System.out.println("认证成功了");
    }
}
```
自定义认证失败处理器：
```
@Component
public class FailureHandlerImpl implements AuthenticationFailureHandler {

    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) throws IOException, ServletException {
        System.out.println("认证失败了");
    }

}
```
自定义登出成功处理器：
```
@Component
public class LogoutSuccessHandlerImpl implements LogoutSuccessHandler {

    @Override
    public void onLogoutSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
        System.out.println("注销成功");
    }

}
```
添加到配置类中：
```
        // 认证成功|失败处理器
        httpSecurity.formLogin().successHandler(successHandler).failureHandler(failureHandler);
        // 登出成功处理器
        httpSecurity.logout().logoutSuccessHandler(logoutSuccessHandler);
```
