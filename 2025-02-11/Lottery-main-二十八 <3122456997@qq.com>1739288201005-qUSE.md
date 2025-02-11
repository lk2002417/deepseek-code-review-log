从提供的 `git diff` 记录来看，代码的改动主要集中在以下几个方面：

### 1. **新增 `BasicMethod` 工具类**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/common/BasicMethod.java`
   - **功能**: 提供了一个静态方法 `stringJudgment`，用于判断字符串是否为空或为 `null`，如果为空或 `null`，则抛出 `IllegalArgumentException`。
   - **评审意见**:
     - 该工具类的设计是合理的，尤其是在处理字符串校验时，能够避免空指针异常。
     - 建议将方法名 `stringJudgment` 改为更具描述性的名称，如 `validateStringNotEmpty`，以提高代码的可读性。
     - 可以考虑将 `IllegalArgumentException` 的异常信息改为更具描述性的内容，例如 `"The input string must not be null or empty"`，以便更好地定位问题。

### 2. **接口 `IActivityRepository` 的修改**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/repository/IActivityRepository.java`
   - **改动**: 新增了 `queryActivityById` 方法，返回类型为 `Optional<Activity>`。
   - **评审意见**:
     - 使用 `Optional` 是一个好的实践，可以避免返回 `null`，减少空指针异常的风险。
     - 建议在接口的注释中明确说明 `Optional` 的使用场景，例如当查询不到数据时返回 `Optional.empty()`。

### 3. **接口 `IAwardRepository` 的修改**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/repository/IAwardRepository.java`
   - **改动**: 将 `queryAwardInfo` 方法的返回类型从 `Award` 改为 `Optional<Award>`，并在注释中明确说明如果查询不到数据则返回 `null`。
   - **评审意见**:
     - 使用 `Optional` 是一个好的改进，但注释中的描述需要更新，因为 `Optional` 不会返回 `null`，而是返回 `Optional.empty()`。
     - 建议将注释修改为 `"如果查询不到该数据则返回 Optional.empty()"`。

### 4. **接口 `IStrategyRepository` 的修改**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/repository/IStrategyRepository.java`
   - **改动**: 将 `queryStrategy` 方法的返回类型从 `Strategy` 改为 `Optional<Strategy>`。
   - **评审意见**:
     - 使用 `Optional` 是一个好的改进，可以减少 `null` 的使用。
     - 建议在接口的注释中明确说明 `Optional` 的使用场景，例如当查询不到数据时返回 `Optional.empty()`。

### 5. **实现类 `ActivityRepository` 的修改**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/repository/impl/ActivityRepository.java`
   - **改动**: 实现了 `queryActivityById` 方法，使用了 `BasicMethod.stringJudgment` 对 `activityId` 进行校验。
   - **评审意见**:
     - 使用 `BasicMethod.stringJudgment` 进行参数校验是一个好的实践，但需要注意 `activityId` 是 `Long` 类型，直接调用 `toString()` 可能会导致不必要的性能开销。
     - 建议在 `BasicMethod` 中增加一个专门处理 `Long` 类型的方法，避免频繁的 `toString()` 调用。

### 6. **实现类 `AwardRepository` 的修改**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/repository/impl/AwardRepository.java`
   - **改动**: 实现了 `queryAwardInfo` 方法，使用了 `BasicMethod.stringJudgment` 对 `awardId` 进行校验，并返回 `Optional<Award>`。
   - **评审意见**:
     - 使用 `Optional` 是一个好的改进，但需要注意 `BasicMethod.stringJudgment` 的异常处理逻辑，确保在异常情况下能够正确处理。

### 7. **实现类 `StrategyDetailRepository` 的修改**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/repository/impl/StrategyDetailRepository.java`
   - **改动**: 在 `queryStrategyDetailList` 方法中使用了 `BasicMethod.stringJudgment` 对 `strategyId` 进行校验，并处理了空列表的情况。
   - **评审意见**:
     - 处理空列表时返回 `Collections.emptyList()` 是一个好的实践，避免了返回 `null`。
     - 建议在方法注释中明确说明当查询不到数据时返回空列表。

### 8. **实现类 `StrategyRepository` 的修改**
   - **文件路径**: `lottery-infrastructure/src/main/java/cn/waitsunshine/lottery/infrastructure/repository/impl/StrategyRepository.java`
   - **改动**: 实现了 `queryStrategy` 方法，使用了 `BasicMethod.stringJudgment` 对 `strategyId` 进行校验，并返回 `Optional<Strategy>`。
   - **评审意见**:
     - 使用 `Optional` 是一个好的改进，但需要注意 `BasicMethod.stringJudgment` 的异常处理逻辑，确保在异常情况下能够正确处理。

### 9. **文件重命名**
   - **文件路径**: `lottery-interfaces/src/main/resources/mybatis/mapper/Activity_Mapper.xml` 重命名为 `ILotteryStrategyRepository.xml`
   - **评审意见**:
     - 文件重命名是一个好的实践，尤其是当文件名与接口名不一致时。确保重命名后的文件名与接口名一致，便于维护和理解。

### 总结
整体来看，代码的改动是合理的，尤其是在使用 `Optional` 和 `BasicMethod` 工具类方面，能够有效减少空指针异常的风险。建议在以下几个方面进行优化：
1. **方法命名**: 将 `stringJudgment` 改为更具描述性的名称，如 `validateStringNotEmpty`。
2. **注释更新**: 在使用 `Optional` 的接口中，明确说明 `Optional` 的使用场景，避免误导。
3. **参数校验**: 在 `BasicMethod` 中增加对 `Long` 类型的校验方法，避免频繁的 `toString()` 调用。
4. **异常信息**: 提供更具描述性的异常信息，便于问题定位。

这些改进将进一步提升代码的可读性和健壮性。