# 工作流（Workflow）

通过计算机对业务流程自动化执行管理



# BPM / BPMN

BPM：Business Process Management，业务流程管理

BPMN：Business Process Model and Notation，业务流程模型与标记



# Activiti

遵循BPMN2.0规范的基于Java的开源流程引擎



### activiti总体架构

- ProcessEngineConfiguration（activiti.cfg.xml）
  - ProcessEngine
    - RepositoryService
    - RuntimeService
    - TaskService
    - HistoryService
    - ManagementService

ProcessEngineConfiguration：流程引擎的配置类，通过该类创建流程引擎

ProcessEngine：流程引擎，提供各种service的接口

RepositoryService：资源管理类，提供了管理和控制流程发布包和流程定义的操作

RuntimeService：运行时管理类，可以获取流程运行时相关的信息

TaskService：任务管理类，获取流程执行中任务相关的信息

HistoryService：历史管理类，查询流程历史数据，包括流程启动时间、任务参与者、执行路径等

ManagementService：流程引擎管理类，主要用于activiti的维护，一般用不到