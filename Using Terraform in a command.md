**ЗАДАНИЕ 1**

TFLINT

только уникальные типы ошибок :

```
11 issue(s) found:

Warning: Module source “git::<https://github.com/udjin10/yandex_compute_instance.git?ref=main>“ uses a default branch as ref (main) (terraform\_module\_pinned\_source)

Warning: Missing version constraint for provider “template” in required\_providers (terraform\_required\_providers)

Warning: [Fixable] variable “default\_cidr” is declared but not used (terraform\_unused\_declarations)

Warning: [Fixable] variable “vpc\_prod” is declared but not used (terraform\_unused\_declarations)
```



CHECKOV
```
“Ensure Terraform module sources use a commit hash”

“Ensure Terraform module sources use a tag with a version number”
```



**ЗАДАНИЕ 2**


2.1
```
git init
git remote add origin git@github.com:BelcEV1985/terraform-05.git
git add .
git commit -m “Initial commit”
git branch -M main
git push -u origin main
```
-----------------------------------------


2.2

terraform init —backend-config=”access\_key== —backend-config=”secret\_key=”
Error: Backend configuration changed

terraform init —backend-config=”access\_key=” —backend-config=”secret\_key=”” -migrate-state


![2 3](https://github.com/user-attachments/assets/13d29008-1ccd-46a7-be41-75bc0d55b42e)


2.3

git commit -m “s3 backet - terraform-05”

-----------------------------------------

2.4

terraform console:

```
Error: Error acquiring the state lock
 
Error message: operation error DynamoDB: PutItem, https response error StatusCode: 400, RequestID:
a733725d-7948-4cf9-9428-7b2d1f249eba, ConditionalCheckFailedException: Condition not satisfied
 
Lock Info:
ID:        76840236-7783-ca65-f46c-ffd4604405c3
Path:      sad-tfstate/terraform.tfstate
Operation: OperationTypeInvalid
Who:       ELECTRO\victorsh@LOGRUS
Version:   1.8.4
Created:   2024-11-20 14:39:19.5853771 +0000 UTC
Info:
Terraform acquires a state lock to protect the state from being written
by multiple users at the same time. Please resolve the issue above and try
again. For most commands, you can disable locking with the "-lock=false"
flag, but this is not recommended.
```

Вывод:

```
terraform force-unlock 76840236-7783-ca65-f46c-ffd4604405c3

 Do you really want to force-unlock?
 `  `Terraform will remove the lock on the remote state.
 `  `This will allow local Terraform commands to modify this state, even though it
 `  `may still be in use. Only 'yes' will be accepted to confirm.
 `  `Enter a value: yes

 Terraform state has been successfully unlocked!

 The state has been unlocked, and Terraform commands should now be able to
 obtain a new lock on the remote state
 ```
 -----------------------------------------


**ЗАДАНИЕ 3**


3.1

git branch terraform-05-hotfix
git checkout terraform-05-hotfix
git push -u origin terraform-05-hotfix

3.2
Исправленные ошибки:

*providers.tf*

Явное указание версии:
source  = "yandex-cloud/yandex"
version = "~> 0.90" # Укажите актуальную версию

*main.tf*

Фиксация версии модуля через hash коммита:
source = "git::https://github.com/udjin10/yandex_compute_instance.git?ref=0049a0c47c805c2552e16f7bca2581a7feae0f14"

**Pull Request**

https://github.com/BelcEV1985/terraform-05/pull/1


**ЗАДАНИЕ 4**

Забыл внести в main.tf тестил сбоку...

```hcl
 variable "ip_address" {
  type        = string
  description = "IP-адрес"
  validation {
    condition     = can(cidrhost(join("/", [var.ip_address, "32"]), 0))
    error_message = "Значение должно быть корректным IP-адресом."
  }
}

variable "ip_address_regex" {
  type        = string
  description = "IP-адрес (проверка через regex)"
  validation {
    condition     = can(regex("^((25[0-5]|(2[0-4]|1\\d|[1-9]|)\\d).?\\b){4}$", var.ip_address_regex))
    error_message = "Значение должно быть корректным IP-адресом."
  }
}
```

![4 1](https://github.com/user-attachments/assets/40f3118b-dd3c-4775-b39d-ce8874216a83)


```hcl
variable "list_addresses" {
  type        = list(string)
  description = "Список IP-адресов"
  validation {
    condition = alltrue([
      for ip in var.list_addresses : can(regex("^((25[0-5]|(2[0-4]|1\\d|[1-9]|)\\d).?\\b){4}$", ip))
    ])
    error_message = "Все значения должны быть корректными IP-адресами."
  }
}
```
![4 2](https://github.com/user-attachments/assets/ad8fb094-1c44-4c8a-b539-ee51acf1572f)

