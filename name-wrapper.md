# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [The ENS Name Wrapper](#the-ens-name-wrapper)
  - [Example - Step By Step](#example---step-by-step)
  - [Example - Parent Locks Subname All-In-One](#example---parent-locks-subname-all-in-one)
  - [Example - Multiple Subnames in Various States](#example---multiple-subnames-in-various-states)
  - [Name Wrapper Fuses](#name-wrapper-fuses)
  - [Name Wrapper Expiry](#name-wrapper-expiry)
  - [Approved Operators](#approved-operators)

## Introduction
Trước khi chúng tôi đi sâu vào **Name Wrapper**, hãy tóm tắt ngắn gọn một số khái niệm ENS cơ bản sẽ cung cấp một số context hữu ích.

**Registry** là contract cốt lõi ở trung tâm của giải pháp ENS. Tất cả các tra cứu ENS bắt đầu bằng cách truy vấn registry. Registry không chỉ dành cho các tên second-level cua .eth (như `name.eth`) mà còn cho tất cả các tên ENS (các tên phụ như `sub.name.eth` và cả các tên DNS như `domain.xyz`). Chủ sở hữu tên trong registry còn được gọi là Controller hoặc Manager của tên.

**.eth Registrar** dành riêng cho đăng ký tên second-level .eth. Đây thực chất là một registrar cho tên miền phụ cho TLD (top-level domain) .eth. Khi bạn đăng ký một tên .eth, registrar sẽ cấp cho bạn một ERC-721 NFT.

Chủ sở hữu của NFT đó còn được gọi là Registrant hoặc Owner của tên đó. Nó cũng có thể được sử dụng để đòi lại quyền sở hữu đối với tên trong core registry (nói cách khác, Registrant có thể ghi đè lên Controller).

![Alt text](image.png)

## The ENS Name Wrapper
## Example - Step By Step
## Example - Parent Locks Subname All-In-One
## Example - Multiple Subnames in Various States
## Name Wrapper Fuses
## Name Wrapper Expiry
## Approved Operators

