## 单元测试

单元测试是用来验证程序单元的正确性，一般由开发人员完成，是测试过程的第一个环节，以确保所写的代码符合软件需求和遵循开发目标。好的单元测试可以带来以下好处：

- 减少了潜在bug，提高代码质量。单元测试可以让人的思考方式不同于编码，能够很快发现不合理的设计或逻辑、算法方面的漏洞，从而为编写高质量代码提供基本保证。
- 大大缩减软件修复的成本。在软件开发生命周期中，越早阶段发现问题或缺陷，修复的代价越低。单元测试阶段发现的问题修复代价远比集成测试或者系统测试阶段的代价低。
- 为集成测试提供保障。单元测试可大大减少程序中各个部件的不可靠性，先测试部件再集成测试，是的集成测试变得更加简单，让测试把更多精力放在用户场景中。



有效的单元测试：

- 测试先行，遵循单元测试步骤。测试不应该是编码结束后再来考虑，在需求阶段就应该考虑。典型的单元测试步骤如下：
  - 创建测试计划
  - 编写测试用例，准备测试数据
  - 编写测试脚本
  - 编写被测代码，在代码完成后执行测试脚本
  - 修正代码缺陷，重新测试直到代码可接受为止
- 遵循单元测试的基本原则：
  - **一致性**：意味着执行1000次和执行一次的效果等价。
  - **原子性**：单元测试的执行结果返回只有两种，True或False。不存在部分成功、部分失败的例子。如果发生这样的情况，说明用例设计不合理。
  - **单一原则**：测试应该基于情景和行为，而不是方法。如果一个方法对应着多个行为，应该有多个测试用例；而一个行为即使对应着多个方法，也只能有一个测试用例。
  - **隔离性**：不能依赖具体的环境设置，如数据库的访问、环境变量的设置、系统的时间；也不能依赖于其他的测试用例以及测试执行的顺序，并且是无条件逻辑依赖。单元测试的所有输入应该是确定的，方法的行为和结果应该是可预测的。
