---
title: "CF 2052A - Cơn sốt adrenaline"
description: "Cuộc đua bắt đầu với những chiếc xe được sắp xếp theo nhãn của chúng: $$1,2,3,dots,n.$$ Trong cuộc đua, một lần vượt là một sự hoán đổi liền kề. Nếu ô tô $x$ ở ngay sau ô tô $y$, thì sự kiện \"$x$ vượt qua $y$\" sẽ hoán đổi vị trí của chúng. Khi kết thúc cuộc đua, chúng ta chỉ biết thứ tự cuối cùng $c$."
date: "2026-06-08T08:34:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 2052
codeforces_index: "A"
codeforces_contest_name: "2024-2025 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1600
weight: 2052
solve_time_s: 204
verified: false
draft: false
---

[CF 2052A - Adrenaline Rush](https://codeforces.com/problemset/problem/2052/A) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cuộc đua bắt đầu với những chiếc xe được đặt hàng theo nhãn của họ:$$1,2,3,\dots,n.$$Trong cuộc đua, vượt là hoán đổi liền kề. Nếu ô tô$x$ở ngay phía sau xe$y$, sau đó là sự kiện "$x$vượt qua$y$" hoán đổi vị trí của họ. 

Kết thúc cuộc đua chúng ta chỉ biết thứ tự cuối cùng$c$. Chúng ta phải tái tạo lại một cuộc đua có càng nhiều lần vượt càng tốt, với một hạn chế: cho mỗi cặp ô tô$(x,y)$, sự kiện "$x$vượt qua$y$" có thể xảy ra nhiều nhất một lần, và sự kiện "$y$vượt qua$x$" có thể xảy ra nhiều nhất một lần. 

Chúng ta cần xuất ra cả số lần vượt tối đa có thể và một chuỗi lần vượt để đạt được số lần vượt đó. 

Ràng buộc$n \le 1000$là nhỏ. Số lượng đôi xe nhiều nhất là$$\frac{1000\cdot999}{2}=499500.$$Vì bản thân câu trả lời có thể chứa$O(n^2)$vượt qua, bất kỳ giải pháp được chấp nhận nào cũng sẽ ít nhất là bậc hai trong trường hợp xấu nhất. MỘT$O(n^2)$thi công hoàn toàn an toàn. 

Một điểm tinh tế là việc tối đa hóa số lần vượt không giống như giảm thiểu hoặc khớp với hoán vị cuối cùng. Đối với một cặp xe có thứ tự tương đối không thay đổi ở thứ hạng cuối cùng, chúng ta có thể cho chúng đổi vị trí hai lần trong suốt chặng đua mà vẫn về đích theo thứ tự ban đầu. Một giải pháp chỉ thực hiện các lần vượt cần thiết để có được hoán vị cuối cùng sẽ bỏ lỡ nhiều lần vượt bổ sung hợp lệ. 

Coi như:```
n = 2
final = [1, 2]
```Lệnh cuối cùng đã là lệnh ban đầu. Một giải pháp ngây thơ sẽ tạo ra số lần vượt qua bằng 0. 

Cuộc đua tối ưu là:```
2 overtakes 1
1 overtakes 2
```Lệnh cuối cùng lại là$[1,2]$, nhưng bây giờ chúng tôi có hai lần vượt qua. 

Một trường hợp dễ bỏ sót khác là:```
n = 3
final = [3,2,1]
```Mỗi cặp đều bị đảo ngược. Không cặp nào có thể đóng góp hai lần vượt, vì việc kết thúc đảo ngược đòi hỏi phải có sự thay đổi thực sự về thứ tự tương đối. Tối đa chính xác là ba lần vượt, một lần cho mỗi cặp. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu từ việc xem xét từng cặp ô tô một cách độc lập. 

Lấy một cặp$(x,y)$với$x<y$. Ban đầu,$x$đi trước$y$. 

Nếu xếp hạng cuối cùng cũng có vị trí$x$trước$y$, thì cặp đôi này có thể đóng góp hai lần vượt:```
y overtakes x
x overtakes y
```Thứ tự tương đối kết thúc không thay đổi. 

Nếu vị trí xếp hạng cuối cùng$y$trước$x$, thì cặp đôi đó có thể đóng góp nhiều nhất một lần vượt:```
y overtakes x
```Thực hiện cả hai hướng sẽ khôi phục trật tự ban đầu, điều này không được phép. 

Điều này ngay lập tức đưa ra một giới hạn trên. Cho phép$P$là số cặp và đặt$I$là số lần đảo ngược của hoán vị cuối cùng so với thứ tự ban đầu. 

Mỗi cặp không đảo ngược đóng góp tối đa hai
