# angular-credit-card-directive
Angular directive for getting the credit card type based on his numbers.

    app.directive('creditCardType', ['$compile', function($compile){
        return {
            restrict: 'AE',
            require: '?ngModel'
            , link: function(scope, elm, attrs, ctrl){
                var mask;
            ctrl.$parsers.unshift(function(value){
                scope.type =
                    (/^5[1-5]/.test(value)) ? "MASTER"
                        : (/^4/.test(value)) ? "VISA"
                        : (/^3[47]/.test(value)) ? 'AMEX'
                        : undefined

                switch(scope.type) {
                    case 'AMEX':
                        mask = '9999 999999 999999?';
                        break;
                    default:
                        mask = '9999 9999 9999 9999?';
                        break;
                };

                if(elm.attr('mask') != mask){
                    elm.removeAttr('credit-card-type');
                    attrs.$set('mask', mask);
                    $compile(elm)(scope);
                }

                return value

            })
        }
      };
    }]

# How to Use
Is necessary to have ngMask to generate the mask (https://github.com/candreoliveira/ngMask). You can simply do this:

    <input type="text" name="cardNumber" ng-model="card.cardNumber" credit-card-type value="">

And get the type like this:

    <pre> {{type}} </pre>
