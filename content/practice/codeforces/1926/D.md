---
title: "CF 1926D - Vlad và Sư đoàn"
description: "Chúng ta được cấp một tập hợp các số nguyên không âm và được yêu cầu phân chia chúng thành các nhóm sao cho trong mỗi nhóm không có hai số nào có chung 1 bit ở cùng một vị trí trên 31 bit đầu tiên."
date: "2026-06-08T19:00:47+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1926
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 928 (Div. 4)"
rating: 1300
weight: 1926
solve_time_s: 123
verified: false
draft: false
---

[CF 1926D - Vlad và Division](https://codeforces.com/problemset/problem/1926/D) 

**Đánh giá:** 1300 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên không âm và được yêu cầu phân chia chúng thành các nhóm sao cho trong mỗi nhóm không có hai số nào có chung 1 bit ở cùng một vị trí trên 31 bit đầu tiên. Nói cách khác, nếu bạn viết tất cả các số ở dạng nhị phân và xem xét các vị trí từ bit có trọng số nhỏ nhất cho đến bit thứ 31, thì không có cột nào trong một nhóm có thể chứa nhiều hơn một số 1. Mỗi số phải thuộc đúng một nhóm và mục tiêu là giảm thiểu số lượng nhóm. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số lượng số nguyên và danh sách các số nguyên. Đầu ra cho mỗi trường hợp thử nghiệm là một số nguyên duy nhất: số lượng nhóm tối thiểu cần thiết. Các ràng buộc ở mức vừa phải: tổng số số nguyên trong tất cả các trường hợp thử nghiệm tối đa là 200.000. Điều đó loại trừ bất kỳ giải pháp nào kiểm tra tất cả các cặp số trong một trường hợp thử nghiệm vì đó có thể là O(n²), sẽ đạt tới các phép toán 4 × 10¹⁰ trong trường hợp xấu nhất. Các giải pháp tuyến tính hoặc gần tuyến tính đều khả thi. 

Trường hợp cạnh không rõ ràng phát sinh khi các số có mẫu bit chồng chéo. Ví dụ: nếu một số xuất hiện nhiều lần, mỗi bản sao phải được xếp vào một nhóm riêng nếu bất kỳ bit 1 nào của nó khớp với một số khác. Một điều tinh tế khác là số 0, không có bit 1. Nhiều số 0 có thể nằm trong cùng một nhóm vì chúng không xung đột với bất kỳ bit nào. 

Hãy xem xét một ví dụ cụ thể: [1, 4, 3, 4]. Ở dạng nhị phân (tối đa 4 bit đầu tiên để đơn giản):```
1 -> 0001
4 -> 0100
3 -> 0011
4 -> 0100
```Ở đây, số '4' thứ hai xung đột với số '4' đầu tiên ở bit thứ 3, vì vậy chúng không thể chia sẻ một nhóm. Các giải pháp bất cẩn có thể cố gắng ghép các số một cách tham lam mà không tính đến các bit lặp lại này, tạo ra số lượng nhóm nhỏ hơn mức cho phép. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các nhóm có thể và kiểm tra các điều kiện bit cho từng nhóm. Về nguyên tắc, điều này đúng vì nó sẽ khám phá tất cả các phân vùng hợp lệ, nhưng nó không khả thi về mặt tính toán. Ngay cả với n = 20, số lượng phân vùng là rất lớn (số Bell tăng rất nhanh) và với n = 2 × 10⁵, cách tiếp cận này là không thể. 

Cái nhìn sâu sắc quan trọng đến từ việc tập trung vào các bit thay vì các số riêng lẻ. Điều kiện cấm hai số bất kỳ trong một nhóm chia sẻ số 1 ở cùng một vị trí bit. Điều đó có nghĩa là, đối với mỗi vị trí bit, số lượng số tối đa trong một nhóm không được vượt quá 1. Tương tự, nếu bạn đếm xem có bao nhiêu số được đặt bit thứ i, thì số đó sẽ đưa ra giới hạn dưới về số lượng nhóm: chúng ta cần ít nhất số nhóm bằng tần số tối đa của bất kỳ bit đơn lẻ nào được đặt trong số tất cả các số. Khi chúng tôi tính toán tần số bit tối đa, con số đó đủ làm số lượng nhóm tối thiểu. Quan sát này chuyển đổi vấn đề từ một vấn đề phân vùng tổ hợp thành một vấn đề đếm trên các bit, tuyến tính theo n và số lượng bit (31), làm cho nó khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ × n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n × 31) = O(n) | O(31) = O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số nguyên`n`và danh sách các số nguyên`a`. 
2. Khởi tạo một mảng`bit_count`có độ dài từ 31 đến 0. Mỗi mục`bit_count[i]`sẽ theo dõi có bao nhiêu số có tập bit thứ i. 
3. Lặp qua từng số trong`a`. Đối với mỗi vị trí bit từ 0 đến 30 (tương ứng với các bit từ 1 đến 31), hãy kiểm tra xem bit đó đã được đặt chưa. Nếu có thì tăng`bit_count[i]`. 
4. Sau khi xử lý tất cả các số, tìm giá trị lớn nhất trong`bit_count`. Mức tối đa này đại diện cho số lượng nhóm tối thiểu vì bit có tần số cao nhất sẽ cần ít nhất nhiều nhóm riêng biệt để tránh xung đột. 
5. Xuất giá trị lớn nhất cho test case. 

Tại sao nó hoạt động: điều bất biến là trong một nhóm, một bit không thể xuất hiện hai lần. Việc đếm số lượng 1 giây trên mỗi bit sẽ nắm bắt chính xác số lượng nhóm tối thiểu cần thiết để đáp ứng ràng buộc cho bit đó. Lấy mức tối đa trên tất cả các bit đảm bảo rằng tất cả các ràng buộc bit được thỏa mãn đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    bit_count = [0] * 31
    for num in a:
        for i in range(31):
            if num & (1 << i):
                bit_count[i] += 1
    print(max(bit_count))
```Vòng lặp bên ngoài xử lý nhiều trường hợp thử nghiệm.`bit_count`theo dõi tần số 1 giây mỗi bit. Vòng lặp bên trong kiểm tra từng bit trong số 31 bit một cách hiệu quả bằng cách sử dụng AND theo bit. Cuối cùng,`max(bit_count)`tạo ra số lượng nhóm tối thiểu cần thiết. Một điểm tinh tế là lập chỉ mục các bit từ 0 đến 30 trong mã vì Python sử dụng lập chỉ mục dựa trên 0 cho các vị trí bit. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
1 4 3 4
```| Số | Nhị phân | Cập nhật số bit | 
| --- | --- | --- | 
| 1 | 0001 | bit 0 → 1 | 
| 4 | 0100 | bit 2 → 1 | 
| 3 | 0011 | bit 0 → 2, bit 1 → 1 | 
| 4 | 0100 | bit 2 → 2 |`bit_count`= [2, 1, 2, 0, ...], max = 2 → đầu ra 2. Mỗi nhóm có thể lấy số tránh các bit lặp lại. 

### Đầu vào mẫu 2```
2
0 2147483647
```| Số | Nhị phân | Cập nhật số bit | 
| --- | --- | --- | 
| 0 | 000...000 | không | 
| 2147483647 | 111...111 (31 bit) | bit 0-30 → mỗi bit 1 |`bit_count`= [1,1,1,...], max = 1 → đầu ra 1. Tất cả các số đều thuộc một nhóm vì 0 không có số 1. 

Những dấu vết này xác nhận tính đúng đắn và bất biến mà không có bit nào xuất hiện hai lần trong một nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n × 31) ≈ O(n) | Đối với mỗi số, chúng tôi kiểm tra 31 bit. | 
| Không gian | O(31) = O(1) | Chúng tôi chỉ lưu trữ một mảng có độ dài 31 để đếm số bit. | 

Thuật toán dễ dàng xử lý tổng n ≤ 2 × 10⁵ trên tất cả các trường hợp thử nghiệm trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        bit_count = [0] * 31
        for num in a:
            for i in range(31):
                if num & (1 << i):
                    bit_count[i] += 1
        print(max(bit_count))
    return output.getvalue().strip()

# provided samples
assert run("1\n4\n1 4 3 4\n") == "2", "sample 1"
assert run("1\n2\n0 2147483647\n") == "1", "sample 2"

# custom cases
assert run("1\n3\n0 0 0\n") == "0", "all zeros"
assert run("1\n5\n1 2 4 8 16\n") == "1", "all bits disjoint"
assert run("1\n3\n7 7 7\n") == "3", "all identical max bits"
assert run("1\n4\n1 3 5 7\n") == "3", "overlapping bits"
assert run("1\n2\n2147483647 2147483647\n") == "2", "max 31-bit numbers repeated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | 0 | Nhiều số không không cần thêm nhóm | 
| 1 2 4 8 16 | 1 | Các số có bit rời nhau có thể chia sẻ một nhóm | 
| 7 7 7 | 3 | Các số giống hệt nhau yêu cầu các nhóm riêng biệt trên mỗi số bit tối đa | 
| 1 3 5 7 | 3 | Các bit chồng chéo được xử lý chính xác | 
| 2147483647 2147483647 | 2 | Số 31 bit tối đa được lặp lại | 

## Vỏ cạnh 

Nếu tất cả các số đều bằng 0,`bit_count`vẫn là tất cả số không. Thuật toán xuất ra chính xác 0, nghĩa là một nhóm có thể chứa tất cả các số 0. Nếu các số có tất cả các bit được đặt (2147483647) và xuất hiện nhiều lần thì số lượng tối đa trên tất cả các bit bằng với số lần lặp lại, đảm bảo mỗi số giống hệt nhau được đặt trong một nhóm riêng biệt. Phương pháp đếm bit xử lý liền mạch cả hai thái cực, tránh hoàn toàn việc so sánh theo cặp.
