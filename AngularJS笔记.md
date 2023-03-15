## 控制器对象

- 可以理解为局部作用域下的响应式属性

  ```html
  <div ng-controller="fn">
  	<input ng-model="firstName">
      <input ng-model="lastName">
  </div>
  <script>
  	function fn($scope){	形参必须写$scope
          $scope.firstName = 'xxx'
          $scope.lastName = 'xxx2'
      }
  </script>