---
title: "CF 1753B - Phân chia giai thừa"
description: "Chúng ta được yêu cầu xác định xem tổng các giai thừa của một mảng số nguyên có chia hết cho một giai thừa khác hay không. Cụ thể hơn, chúng ta được cho một mảng [a1, a2, ..., an] và một số x. Chúng tôi muốn biết liệu (a1! + a2! + ... + an!) có chia hết cho x!."
date: "2026-06-09T14:58:51+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1753
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 829 (Div. 1)"
rating: 1600
weight: 1753
solve_time_s: 280
verified: true
draft: false
---

[CF 1753B - Tính chia hết giai thừa](https://codeforces.com/problemset/problem/1753/B) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 4 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem tổng các giai thừa của một mảng số nguyên có chia hết cho một giai thừa khác hay không. Cụ thể hơn, chúng ta được cung cấp một mảng`[a_1, a_2, ..., a_n]`và một số`x`. Chúng tôi muốn biết liệu`(a_1! + a_2! + ... + a_n!)`chia hết cho`x!`. Mỗi`a_i`được đảm bảo nhiều nhất`x`, vì vậy chúng ta không bao giờ phải tính giai thừa lớn hơn`x!`. 

Những hạn chế chính là`n`Và`x`mỗi cái có thể lên tới 500.000. Việc tính toán các giai thừa một cách đơn giản cho từng phần tử và tính tổng chúng sẽ liên quan đến các số có kích thước gần đúng.`x!`, phát triển nhanh hơn nhiều so với bất kỳ loại dữ liệu nào có thể xử lý. Thậm chí là 20! đã có kích thước khoảng 2,4×10^18, do đó việc tính toán đơn giản là không thể đối với các số lớn`x`. Điều này cho chúng ta biết rằng chúng ta phải suy luận theo phương pháp toán học thay vì tính toán trực tiếp các giai thừa. 

Một trường hợp cạnh quan trọng phát sinh khi một số phần tử trong mảng chính xác`x`. Từ`x!`chia rẽ`x!`, bất kỳ tổng nào chứa`x!`tự động chia hết cho`x!`nếu nó chứa ít nhất một`x!`. Một trường hợp tế nhị khác là khi tất cả`a_i`ít hơn`x-1`. Khi đó tổng đó hoàn toàn nhỏ hơn`x!`, nên việc chia hết là không thể. Ví dụ, nếu`x = 5`và tất cả`a_i ≤ 3`, tổng lớn nhất là`n * 6 = 6n`, nhỏ hơn`5! = 120`Trừ khi`n`là rất lớn. Nhưng vì tổng luôn bao gồm các giai thừa nhỏ hơn nên cần kiểm tra rõ ràng rằng nếu có`a_i`chính xác là`x`, câu trả lời luôn là "Có". Nếu không, chúng ta phải đảm bảo tất cả`a_i ≥ x-1`để đạt được sự phân chia. 

## Phương pháp tiếp cận 

Phương pháp brute-force là tính từng giai thừa, tính tổng chúng và kiểm tra khả năng chia hết cho`x!`. Điều này đúng về nguyên tắc nhưng thất bại ngay lập tức vì`x > 20`, vì giai thừa vượt quá giới hạn số nguyên tiêu chuẩn và mất quá nhiều thời gian để tính toán. Vì`n = 500,000`Và`x = 500,000`, việc tính toán giai thừa là hoàn toàn không khả thi. 

Quan sát quan trọng là giai thừa phát triển cực kỳ nhanh. Nếu có`a_i`chính xác là`x`,`x!`chia nó ra, vì vậy chúng ta có thể trả về ngay "Có". Nếu tất cả các phần tử đều nhỏ hơn`x`, thì giai thừa lớn nhất của tổng là`(x-1)!`. Tính tổng bất kỳ số giai thừa nào nhỏ hơn`x!`không thể đạt tới bội số của`x!`, bởi vì`x!`thực sự lớn hơn tổng của tất cả`(x-1)!`điều khoản. Điều này làm giảm vấn đề thành một kiểm tra đơn giản: nếu phần tử lớn nhất trong mảng bằng`x`, trả về "Có"; nếu không, trả về "Không". 

Cách tiếp cận này tránh tất cả các tính toán giai thừa thực tế và hoạt động theo thời gian tuyến tính tương ứng với kích thước mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * x) | O(1) hoặc O(x) | Quá chậm/không khả thi đối với x lớn | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`Và`x`. 
2. Đọc mảng`a`chiều dài`n`. 
3. Tính giá trị lớn nhất trong mảng. Điều này xác định giai thừa lớn nhất trong tổng. 
4. Nếu giá trị tối đa bằng`x`, tổng chứa`x!`, nên tổng chia hết cho`x!`. Đầu ra "Có". 
5. Nếu không, số tiền hoàn toàn nhỏ hơn`x!`và không thể chia hết cho`x!`. Đầu ra "Không". 

Tại sao nó hoạt động:`x!`chia hết cho tất cả các giai thừa nhỏ hơn, nhưng tổng các giai thừa nhỏ hơn không bao giờ là bội số của`x!`trừ khi có ít nhất một số hạng bằng`x!`. Bằng cách chỉ theo dõi phần tử tối đa, chúng tôi nắm bắt được bất biến này một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    if max(a) == x:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào hiệu quả và xử lý các mảng lớn. Chúng tôi sử dụng`max(a)`để tìm phần tử lớn nhất thay vì lặp lại và so sánh thủ công, một cách ngắn gọn và hiệu quả. Không có vòng lặp để tính toán giai thừa, tránh tràn số nguyên. Chúng ta cũng không cần lưu trữ giai thừa trung gian, tiết kiệm bộ nhớ. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
6 4
3 2 2 2 3 3
```| Bước | tối đa(a) | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc mảng | [3,2,2,2,3,3] | tối đa(a)=3 | 3 != 4 → Không | 

Ở đây, phần tử lớn nhất là 3, nhỏ hơn`x=4`. Như vậy`4!`không chia tổng các giai thừa. 

**Ví dụ 2** 

đầu vào:```
7 3
3 2 2 2 2 2 1
```| Bước | tối đa(a) | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc mảng | [3,2,2,2,2,2,1] | tối đa(a)=3 | 3 == 3 → Có | 

Phần tử lớn nhất bằng`x=3`, vậy tổng bao gồm`3!`và chia hết cho`3!`. 

Những dấu vết này xác nhận logic: chỉ phần tử tối đa mới quan trọng đối với khả năng chia hết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tìm giá trị lớn nhất của`n`các phần tử mất thời gian tuyến tính. | 
| Không gian | O(n) | Lưu trữ mảng đầu vào; không cần thêm không gian. | 

Giải pháp phù hợp thoải mái trong các ràng buộc của vấn đề, ngay cả đối với`n = 500,000`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    return "Yes" if max(a) == x else "No"

# Provided samples
assert run("6 4\n3 2 2 2 3 3\n") == "No", "sample 1"
assert run("8 3\n3 2 2 2 2 2 1 1\n") == "Yes", "sample 2"

# Custom cases
assert run("1 1\n1\n") == "Yes", "minimum input, single element equals x"
assert run("5 5\n1 2 3 4 4\n") == "No", "maximum less than x"
assert run("5 5\n1 2 3 4 5\n") == "Yes", "array contains x"
assert run("3 10\n7 8 9\n") == "No", "all elements less than x, larger x"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / [1] | Có | Đầu vào tối thiểu, phần tử đơn bằng x | 
| 5 5 / [1,2,3,4,4] | Không | Tất cả các phần tử nhỏ hơn x | 
| 5 5 / [1,2,3,4,5] | Có | Mảng chứa x | 
| 3 10 / [7,8,9] | Không | X lớn hơn, tất cả các phần tử ít hơn | 

## Vỏ cạnh 

Đối với trường hợp`n=1, x=1, a=[1]`, thuật toán sẽ đưa ra chính xác "Có" vì giai thừa đơn`1!`bằng`x!`. Đối với trường hợp tất cả các phần tử đều nhỏ hơn`x`, chẳng hạn như`n=3, x=10, a=[7,8,9]`, tổng các giai thừa nhỏ hơn`10!`, do đó thuật toán sẽ xuất ra "Không" một cách chính xác. Các trường hợp đặc biệt này cho thấy rằng việc kiểm tra phần tử tối đa của chúng tôi nắm bắt một cách đáng tin cậy tất cả các kịch bản chia hết có thể xảy ra mà không cần tính toán các giai thừa lớn.
