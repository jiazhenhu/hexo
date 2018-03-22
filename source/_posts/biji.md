---
title: SSM动态修改定时器
date: 2018-03-20 08:36:33
tags: SSM
---
	@Component
    @EnableScheduling
    public class updateCronTask implements SchedulingConfigurer {
        public static String cron = "0/2 * * * * ?";
        int i=0;
        @Override
        public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
            taskRegistrar.addTriggerTask(new Runnable() {
            @Override
            public void run(){
            i++;
                // 任务逻辑
            System.out.println("第"+(i)+"次开始执行操作... " +"时间：【" + new SimpleDateFormat("yyyyMMdd hh:mm:ss.SSS").format(new Date()) + "】");
            }
        }, new Trigger(){
            @Override
            public Date nextExecutionTime(TriggerContext triggerContext) {
                //任务触发，可修改任务的执行周期
                CronTrigger trigger = new CronTrigger(cron);
                Date nextExec = trigger.nextExecutionTime(triggerContext);
                return nextExec;
            }
        });
    }
	}
