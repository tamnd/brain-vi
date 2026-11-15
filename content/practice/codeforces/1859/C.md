---
title: "CF 1859C - Một vấn đề hoán vị khác"
description: "Chúng ta được hoán vị các số từ 1 đến n. Đối với hoán vị p, hãy xác định $$text{cost}(p)=sum{i=1}^{n} pi cdot i-max{i=1}^{n}(picdot i).$$ Nhiệm vụ là tìm giá trị lớn nhất có thể có của biểu thức này trong số tất cả các hoán vị có độ dài n."
date: "2026-06-09T00:27:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1859
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 892 (Div. 2)"
rating: 1200
weight: 1859
solve_time_s: 168
verified: true
draft: false
---

[CF 1859C - Một vấn đề hoán vị khác](https://codeforces.com/problemset/problem/1859/C) 

**Đánh giá:** 1200 
**Tags:** vũ lực, dp, tham lam, toán học 
**Thời gian giải:** 2 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ`1`ĐẾN`n`. Đối với một hoán vị`p`, định nghĩa$$\text{cost}(p)=\sum_{i=1}^{n} p_i \cdot i-\max_{i=1}^{n}(p_i\cdot i).$$Nhiệm vụ là tìm giá trị lớn nhất có thể có của biểu thức này trong số tất cả các hoán vị có độ dài`n`. 

Thuật ngữ đầu tiên thưởng cho việc đặt số lượng lớn ở các chỉ số lớn vì đây là tổng có trọng số. Thuật ngữ thứ hai trừ đi phần đóng góp lớn nhất. Hai mục tiêu này cạnh tranh với nhau. Chúng tôi muốn tổng trọng số phải lớn, nhưng chúng tôi cũng muốn sản phẩm riêng lẻ lớn nhất không trở nên quá nổi trội. 

Ràng buộc`n ≤ 250`là nhỏ bất thường đối với một bài toán hoán vị. Việc tìm kiếm mạnh mẽ trên tất cả các hoán vị là không thể bởi vì có`n!`hoán vị. Ngay cả đối với`n = 10`, đã có hơn ba triệu hoán vị. Mặt khác, giá trị nhỏ của`n`gợi ý rằng một`O(n²)`hoặc thậm chí`O(n³)`giải pháp cho mỗi trường hợp thử nghiệm là hoàn toàn có thể chấp nhận được. 

Một điểm tinh tế là việc tối đa hóa tổng trọng số một mình là điều dễ dàng. Theo bất đẳng thức sắp xếp lại, giá trị lớn nhất của$$\sum p_i i$$thu được bằng hoán vị được sắp xếp`[1,2,...,n]`. Tuy nhiên, sự sắp xếp đó cũng tạo ra sản phẩm lớn nhất có thể`n²`, sau đó được trừ đi. Câu trả lời tối ưu đến từ việc cân bằng hai tác động này. 

Coi như`n = 4`. 

Hoán vị danh tính mang lại$$1\cdot1+2\cdot2+3\cdot3+4\cdot4=30$$và sản phẩm tối đa là`16`, vậy chi phí là`14`. 

Hoán vị tối ưu là`[1,2,4,3]`, chi phí của nó là`17`. 

Một giải pháp chỉ tối đa hóa tổng trọng số sẽ bỏ lỡ mức tối ưu. 

Một sai lầm dễ mắc phải khác là cho rằng tích lớn nhất phải xảy ra ở vị trí`n`. Ví dụ, trong`[1,4,2,3]`, sản phẩm lớn nhất là`4·2 = 8`, không nhất thiết là vị trí cuối cùng. Bất kỳ giải pháp đúng nào cũng phải tính toán sản phẩm tối đa một cách rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là liệt kê mọi hoán vị, tính tổng có trọng số và tích lớn nhất của nó, sau đó giữ lại câu trả lời đúng nhất. 

Điều này hiệu quả vì việc xác định chi phí rất đơn giản. Đối với một hoán vị cố định, việc đánh giá biểu thức cần`O(n)`thời gian. 

Vấn đề là số lượng hoán vị. Trường hợp xấu nhất cần phải kiểm tra`250!`hoán vị, một con số lớn đến mức hoàn toàn không thể thực hiện được. 

Quan sát chính là giải pháp chính thức dựa trên một không gian tìm kiếm rất nhỏ gồm các hoán vị có cấu trúc đặc biệt. 

Giả sử chúng ta bắt đầu từ hoán vị tăng dần$$[1,2,3,\dots,n].$$Sự sắp xếp này tối đa hóa tổng trọng số. Để giảm hình phạt từ sản phẩm lớn nhất, chúng ta có thể di chuyển một số giá trị lớn ra khỏi chỉ số lớn nhất. 

Một thực tế đáng chú ý là luôn có thể đạt được hoán vị tối ưu bằng cách chọn vị trí phân chia`k`và xây dựng$$[1,2,\dots,k-1,n,n-1,\dots,k].$$Tiền tố vẫn tăng, trong khi hậu tố bị đảo ngược. 

Từ`n ≤ 250`, chúng ta có thể thử mọi vị trí phân chia có thể. Đối với mỗi hoán vị ứng cử viên, chúng tôi tính toán trực tiếp chi phí của nó và lấy mức tối đa. 

chỉ có`n`hoán vị ứng cử viên, mỗi hoán vị yêu cầu`O(n)`làm việc để đánh giá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các hoán vị | O(n·n!) | O(n) | Quá chậm | 
| Liệt kê các vị trí phân chia | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, khởi tạo câu trả lời bằng 0. 
2. Đối với mọi vị trí phân chia`k`từ`1`ĐẾN`n`, xây dựng hoán vị:$$[1,2,\dots,k-1,n,n-1,\dots,k].$$3. Tính tổng có trọng số$$\sum p_i\cdot i.$$4. Tính giá trị lớn nhất trong số tất cả các sản phẩm`p_i · i`. 
5. Chi phí của hoán vị này là tổng có trọng số trừ đi tích lớn nhất. 
6. Cập nhật câu trả lời với chi phí tối đa được thấy cho đến nay. 
7. Sau khi kiểm tra tất cả các vị trí phân chia, hãy in câu trả lời. 

### Tại sao nó hoạt động 

Hoán vị tăng dần sẽ tối đa hóa tổng trọng số. Bất kỳ giải pháp tối ưu nào cũng phải bám sát sự sắp xếp đó vì mất quá nhiều tổng trọng số sẽ rất tốn kém. 

Quyền tự do đáng kể duy nhất là quyết định những giá trị lớn nào sẽ được chuyển ra khỏi các chỉ số lớn nhất để giảm tích tối đa. Đảo ngược hậu tố sẽ đạt được chính xác sự cân bằng đó. Quan sát biên tập cho vấn đề này là mọi giải pháp tối ưu đều thuộc về họ hoán vị đảo ngược hậu tố này. 

Vì chúng tôi đánh giá rõ ràng mọi thành viên trong họ đó nên chi phí tối đa được tìm thấy là mức tối ưu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans_list = []

    for _ in range(t):
        n = int(input())
        best = 0

        for k in range(1, n + 1):
            p = list(range(1, k))
            p.extend(range(n, k - 1, -1))

            total = 0
            mx = 0

            for i, x in enumerate(p, start=1):
                prod = i * x
                total += prod
                if prod > mx:
                    mx = prod

            best = max(best, total - mx)

        ans_list.append(str(best))

    sys.stdout.write("\n".join(ans_list))

solve()
```Vòng lặp bên ngoài lặp qua các trường hợp thử nghiệm. Đối với mỗi vị trí phân chia`k`, chúng tôi tạo ra hoán vị ứng cử viên được mô tả ở trên. 

Việc tính toán chi phí được thực hiện trong một lần quét. Chúng tôi tích lũy số tiền có trọng số và đồng thời theo dõi sản phẩm lớn nhất. 

Câu trả lời tối đa trên tất cả các ứng cử viên được báo cáo. 

Việc triển khai sử dụng lập chỉ mục dựa trên 1 thông qua`enumerate(..., start=1)`vì công thức sử dụng các vị trí được đánh số từ một. 

Tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. Tổng trọng số lớn nhất là theo thứ tự$$250 \cdot 250^2 = 15{,}625{,}000,$$rất nhỏ so với giới hạn số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```Có hai hoán vị ứng cử viên. 

| k | Hoán vị | Tổng có trọng số | Sản phẩm tối đa | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [2,1] | 4 | 2 | 2 | 
| 2 | [1,2] | 5 | 4 | 1 | 

Chi phí tốt nhất là`2`. 

Đầu ra:```
2
```Ví dụ này cho thấy hoán vị danh tính không phải lúc nào cũng tối ưu. 

### Ví dụ 2 

đầu vào:```
4
```| k | Hoán vị | Tổng có trọng số | Sản phẩm tối đa | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [4,3,2,1] | 20 | 6 | 14 | 
| 2 | [1,4,3,2] | 25 | 9 | 16 | 
| 3 | [1,2,4,3] | 29 | 12 | 17 | 
| 4 | [1,2,3,4] | 30 | 16 | 14 | 

Giá trị tối đa là`17`. 

Đầu ra:```
17
```Điều này thể hiện sự đánh đổi trung tâm. Hoán vị nhận dạng tối đa hóa tổng có trọng số, nhưng cách sắp xếp được sửa đổi một chút sẽ giảm hình phạt đủ để tạo ra câu trả lời cuối cùng lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) cho mỗi trường hợp thử nghiệm |`n`chia vị trí, mỗi vị trí được đánh giá trong`O(n)`| 
| Không gian | O(n) | Lưu trữ một hoán vị ứng cử viên | 

Giá trị tối đa của`n`chỉ là`250`, và tổng của tất cả`n`giá trị nhiều nhất là`500`. MỘT`O(n²)`giải pháp thực hiện tối đa khoảng`125,000`hoạt động trên tất cả các ứng cử viên, điều này có thể dễ dàng
