有些业务可能需要在事务提交之后才能执行，可以使用TransactionSynchronizationManager来实现

public void afterCommitProcess() throws Exception {
    TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronizationAdapter() {
        @Override
        public void afterCommit() {
            System.out.println("after transaction commit...");
        }
    });
}