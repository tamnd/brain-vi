---
title: "CF 339B - Xenia và Đường vành đai"
description: "Chúng ta có một thành phố hình tròn với n ngôi nhà xếp theo chiều kim đồng hồ dọc theo một đường vành đai. Mỗi ngôi nhà có một số duy nhất từ ​​1 đến n và giao thông chỉ di chuyển theo chiều kim đồng hồ. Xenia bắt đầu ở nhà 1 và có m nhiệm vụ phải hoàn thành theo thứ tự cụ thể, mỗi nhiệm vụ nằm ở số nhà a[i]."
date: "2026-06-06T17:08:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 339
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 197 (Div. 2)"
rating: 1000
weight: 339
solve_time_s: 115
verified: false
draft: false
---

[CF 339B - Xenia và Đường vành đai](https://codeforces.com/problemset/problem/339/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thành phố hình tròn với`n`những ngôi nhà được bố trí theo chiều kim đồng hồ dọc theo đường vành đai. Mỗi ngôi nhà có một số duy nhất từ ​​1 đến`n`và lưu lượng truy cập chỉ theo chiều kim đồng hồ. Xenia bắt đầu ở nhà 1 và có`m`nhiệm vụ cần hoàn thành theo một thứ tự cụ thể, mỗi nhiệm vụ nằm ở một số nhà`a[i]`. Thời gian để di chuyển từ nhà này sang nhà hàng xóm ngay cạnh theo chiều kim đồng hồ là 1 đơn vị. Mục tiêu là tính tổng thời gian Xenia sẽ dành để đi du lịch để hoàn thành tất cả các nhiệm vụ theo thứ tự nhất định. 

Đầu vào bao gồm hai số nguyên`n`Và`m`, theo sau là`m`các số nguyên biểu thị dãy nhà mà Xenia phải ghé thăm. Đầu ra là một số nguyên duy nhất, tổng thời gian để hoàn thành tất cả các nhiệm vụ. 

Với các ràng buộc (`2 ≤ n ≤ 10^5`Và`1 ≤ m ≤ 10^5`) và giới hạn thời gian là 2 giây, mọi thuật toán đều phải chạy trong thời gian gần như tuyến tính. Các vòng lặp lồng nhau hoặc phương pháp bậc hai, sẽ thực hiện tối đa`10^10`các hoạt động trong trường hợp xấu nhất là quá chậm. Mỗi nước đi có thể được tính toán trong thời gian không đổi, vì vậy chúng ta có thể nhắm đến một`O(m)`giải pháp. 

Các trường hợp có biên không rõ ràng bao gồm các tình huống trong đó nhiệm vụ tiếp theo là ở một ngôi nhà có số lượng nhỏ hơn nhiệm vụ hiện tại. Ví dụ: nếu Xenia ở nhà 3 trong vòng 4 nhà và nhiệm vụ tiếp theo là ở nhà 2, cô ấy phải đi qua nhà 4 rồi đến nhà 1 trước khi đến nhà 2. Một cách tiếp cận ngây thơ chỉ trừ các chỉ số sẽ cho thời gian di chuyển âm không chính xác. Một trường hợp đặc biệt khác là các nhiệm vụ liên tiếp trong cùng một ngôi nhà sẽ không mất thêm thời gian. Ví dụ, trong`n = 5`, nhiệm vụ`[3, 3, 2]`, thời gian để hoàn thành nhiệm vụ thứ hai lúc 3 là 0, khi đó cô ấy di chuyển từ 3 đến 2 dọc theo vòng tròn, lấy 4 đơn vị chứ không phải 2. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng Xenia di chuyển từng bước từ nhà này sang nhà khác dọc theo đường vành đai. Đối với mỗi nhiệm vụ, chúng tôi sẽ tăng bộ đếm cho mỗi ngôi nhà đi qua. Mặc dù điều này đúng về mặt khái niệm, nhưng trường hợp xấu nhất xảy ra khi Xenia phải di chuyển gần như toàn bộ vòng đấu cho mỗi nhiệm vụ, dẫn đến`O(m * n)`hoạt động xung quanh`10^10`cho những hạn chế tối đa và quá chậm. 

Quan sát chính là việc di chuyển dọc theo đường vòng một chiều cho phép chúng ta tính toán thời gian di chuyển giữa hai ngôi nhà bằng cách sử dụng số học đơn giản thay vì mô phỏng từng bước. Nếu Xenia ở nhà`current`và cần đạt được`next`, khoảng cách dọc theo vòng là`next - current`nếu như`next >= current`, hoặc`n - (current - next)`nếu như`next < current`. Điều này giải thích cho việc quấn quanh chiếc nhẫn. Điều này biến mô phỏng thành một lần tuyến tính duy nhất trên danh sách nhiệm vụ, tính toán khoảng cách trong thời gian không đổi cho mỗi nhiệm vụ. 

Bằng cách cộng các khoảng cách này, chúng ta có thể tính được tổng thời gian trong`O(m)`không có thêm không gian ngoài các biến theo dõi ngôi nhà hiện tại và tổng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng từng bước) | O(m * n) | O(1) | Quá chậm | 
| Tối ưu (tính toán khoảng cách số học) | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`current`đến 1, đại diện cho nhà xuất phát của Xenia và`total_time`đến 0. Chúng tôi theo dõi`current`bởi vì nhiệm vụ tiếp theo phụ thuộc vào ngôi nhà mà Xenia hiện đang ở. 
2. Lặp lại từng nhiệm vụ`task`theo trình tự đã cho. Đối với mỗi`task`, xác định thời điểm chuyển động từ`current`ĐẾN`task`. Nếu như`task >= current`, khoảng cách là`task - current`. Nếu không cô ấy phải quấn quanh chiếc nhẫn nên khoảng cách là`n - (current - task)`. Thêm khoảng cách này vào`total_time`. 
3. Cập nhật`current`ĐẾN`task`để phản ánh vị trí mới của Xenia. 
4. Sau khi xử lý xong tất cả các tác vụ, hãy in`total_time`. 

Tại sao nó hoạt động: bất biến được duy trì là`current`luôn phản ánh chính xác ngôi nhà hiện tại của Xenia trước khi chuyển sang nhiệm vụ tiếp theo. Việc tính toán khoảng cách xử lý chính xác tính chất vòng tròn của con đường bằng cách phân biệt các trường hợp nhiệm vụ tiếp theo ở phía trước hoặc phía sau về mặt đánh số nhà. Tổng các khoảng cách này để đảm bảo rằng tất cả thời gian di chuyển được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
tasks = list(map(int, input().split()))

current = 1
total_time = 0

for task in tasks:
    if task >= current:
        total_time += task - current
    else:
        total_time += n - (current - task)
    current = task

print(total_time)
```Dòng đầu tiên đọc quy mô thành phố và số lượng nhiệm vụ. Dòng thứ hai đọc trình tự các nhiệm vụ. Chúng tôi khởi tạo`current`đến 1, ngôi nhà bắt đầu, và`total_time`đến 0. Vòng lặp lặp lại các nhiệm vụ và tính toán khoảng cách tùy thuộc vào việc số nhà nhiệm vụ ở phía trước hay yêu cầu bao quanh. Đang cập nhật`current`đảm bảo lần lặp tiếp theo sử dụng điểm bắt đầu chính xác. Cách tiếp cận này tránh được từng lỗi một vì số nhà bắt đầu từ 1. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
4 3
3 2 3
```| Nhiệm vụ | Hiện tại | Khoảng cách | Tổng_thời gian | 
| --- | --- | --- | --- | 
| 3 | 1 | 2 | 2 | 
| 2 | 3 | 3 | 5 | 
| 3 | 2 | 1 | 6 | 

Xenia di chuyển 1→2→3 (2 đơn vị), quấn quanh 3→4→1→2 (3 đơn vị), sau đó 2→3 (1 đơn vị). Tổng thời gian là 6. 

**Mẫu 2** 

đầu vào:```
5 4
2 2 4 1
```| Nhiệm vụ | Hiện tại | Khoảng cách | Tổng_thời gian | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 1 | 
| 4 | 2 | 2 | 3 | 
| 1 | 4 | 2 | 5 | 

Điều này chứng tỏ các nhiệm vụ liên tiếp trong cùng một ngôi nhà không mất thời gian và logic bao quanh từ 4→1 được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi tác vụ được xử lý một lần với số học theo thời gian không đổi. | 
| Không gian | O(m) | Chúng tôi lưu trữ danh sách nhiệm vụ; không cần cấu trúc bổ sung. | 

Điều này phù hợp thoải mái trong các ràng buộc:`m`Và`n`lên tới 10^5 và mỗi phép tính số học đều không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    tasks = list(map(int, input().split()))
    current = 1
    total_time = 0
    for task in tasks:
        if task >= current:
            total_time += task - current
        else:
            total_time += n - (current - task)
        current = task
    return str(total_time)

# provided sample
assert run("4 3\n3 2 3\n") == "6", "sample 1"

# custom tests
assert run("5 4\n2 2 4 1\n") == "5", "consecutive tasks & wrap"
assert run("2 5\n1 2 1 2 1\n") == "5", "small n, alternating"
assert run("10 1\n10\n") == "9", "single task at last house"
assert run("3 3\n3 3 3\n") == "2", "all tasks same house"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 4\n2 2 4 1 | 5 | Nhiệm vụ liên tiếp và logic bao quanh | 
| 2 5\n1 2 1 2 1 | 5 | Vòng nhỏ, nhiệm vụ xen kẽ, truyền tải đầy đủ | 
| 10 1\n10 | 9 | Nhiệm vụ duy nhất tại ngôi nhà cuối cùng | 
| 3 3\n3 3 3 | 2 | Nhiệm vụ lặp đi lặp lại trong cùng một ngôi nhà, lặp lại khoảng cách bằng 0 | 

## Vỏ cạnh 

Đối với các nhiệm vụ theo thứ tự giảm dần trên vòng, thuật toán sẽ kết thúc chính xác. đầu vào`n=4, tasks=[4,1,2]`kết quả là`3+1+1=5`đơn vị: 1→2→3→4 (3 đơn vị), gói 4→1 (1 đơn vị), 1→2 (1 đơn vị). Đối với các nhiệm vụ lặp đi lặp lại trong cùng một ngôi nhà, khoảng cách bằng 0, ví dụ:`n=5, tasks=[2,2]`sản lượng`1`đơn vị. Trong cả hai trường hợp, việc tính toán khoảng cách số học đảm bảo thời gian di chuyển chính xác mà không cần mô phỏng từng bước từng ngôi nhà.
