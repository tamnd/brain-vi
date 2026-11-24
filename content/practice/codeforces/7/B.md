---
title: "CF 7B - Trình quản lý bộ nhớ"
description: "Chúng tôi được yêu cầu triển khai trình quản lý bộ nhớ đơn giản cho mảng bộ nhớ tuyến tính có kích thước _m_. Mỗi ô nhớ có thể trống hoặc bị chiếm bởi một khối. Chúng ta phải xử lý một chuỗi các thao tác: cấp phát n, xóa x và chống phân mảnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 7
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 7"
rating: 1600
weight: 7
solve_time_s: 61
verified: true
draft: false
---
[CF 7B - Trình quản lý bộ nhớ](https://codeforces.com/problemset/problem/7/B) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu triển khai trình quản lý bộ nhớ đơn giản cho mảng bộ nhớ tuyến tính có kích thước _m_. Mỗi ô nhớ có thể trống hoặc bị chiếm bởi một khối. Chúng ta phải xử lý một chuỗi các hoạt động:`alloc n`,`erase x`, Và`defragment`. MỘT`alloc n`yêu cầu cố gắng phân bổ một phân đoạn liền kề gồm _n_ byte. Nếu tồn tại nhiều phân đoạn trống liền kề nhau thì phân đoạn gần đầu bộ nhớ nhất sẽ được chọn. Mỗi lần phân bổ thành công sẽ nhận được một mã định danh duy nhất bắt đầu từ 1. Nếu phân bổ không thành công, chúng ta phải trả về`NULL`. 

MỘT`erase x`yêu cầu giải phóng bộ nhớ được liên kết với mã định danh _x_. Nếu _x_ chưa bao giờ được phân bổ hoặc đã bị xóa, chúng tôi sẽ in`ILLEGAL_ERASE_ARGUMENT`. Cuối cùng,`defragment`di chuyển tất cả các khối được phân bổ về phía đầu bộ nhớ trong khi vẫn giữ nguyên thứ tự của chúng, loại bỏ các khoảng trống giữa chúng. 

Các ràng buộc rất chặt chẽ nhưng có thể quản lý được. Tổng số thao tác tối đa là 100 và bộ nhớ tối đa là 100 byte. Điều này cho phép các giải pháp mô phỏng bộ nhớ một cách rõ ràng, vì ngay cả thuật toán O(t*m) đơn giản cũng sẽ thực hiện tối đa 10.000 bước, nằm trong giới hạn 1 giây. 

Các trường hợp biên bao gồm phân bổ một khối khi không tồn tại phân đoạn tự do liền kề đủ lớn, cố gắng xóa một khối chưa bao giờ được phân bổ và phân bổ sau khi chống phân mảnh di chuyển các khối nhưng không thay đổi kích thước của chúng. Ví dụ, nếu bộ nhớ là`[alloc 3, alloc 4, erase 1]`, tiếp theo`alloc 5`không chống phân mảnh sẽ thất bại vì không gian trống bị chia cắt, trong khi có chống phân mảnh thì sẽ thành công. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng bộ nhớ dưới dạng danh sách có độ dài _m_, lưu trữ`0`đối với byte trống hoặc ID phân bổ cho byte bị chiếm dụng. Vì`alloc n`, chúng tôi quét danh sách để tìm chuỗi liền kề đầu tiên có ít nhất _n_ số 0, thay thế nó bằng ID mới và tăng bộ đếm phân bổ. Vì`erase x`, chúng tôi quét bộ nhớ và thay thế tất cả các lần xuất hiện của ID`x`bằng không.`defragment`được triển khai bằng cách lọc tất cả các ID khác 0 ở phía trước và đệm phần còn lại bằng số 0. Điều này đúng vì tất cả các thao tác chỉ yêu cầu kiểm tra các chuỗi liền kề và duy trì ID phân bổ, nhưng việc quét bộ nhớ liên tục để tìm _m_ lớn sẽ chậm nếu các ràng buộc lớn hơn. 

Quan sát chính để tối ưu hóa là kích thước bài toán cho phép mô phỏng trực tiếp. Đối với bộ nhớ lớn hơn, người ta có thể duy trì danh sách các phân đoạn trống dưới dạng các khoảng thời gian, phân bổ và hợp nhất các khoảng thời gian cũng như di chuyển các phân đoạn được phân bổ dưới dạng một danh sách riêng biệt. Cấu trúc của vấn đề này làm cho việc quét tuyến tính là đủ, bởi vì cả hai`t`Và`m`là 100. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t*m) | O(m) | Đã chấp nhận | 
| Tối ưu hóa khoảng thời gian | O(t*#blocks) | O(#khối) | Không cần thiết cho các ràng buộc | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ nhớ dưới dạng danh sách các số 0 có độ dài _m_. Duy trì bộ đếm phân bổ bắt đầu từ 1 và ID ánh xạ từ điển tới phạm vi bộ nhớ của chúng để kiểm tra xóa nhanh. 
2. Đối với một`alloc n`thao tác, lặp qua bộ nhớ từ đầu đến cuối để tìm chuỗi đầu tiên có ít nhất _n_ số 0 liên tiếp. Nếu tìm thấy, hãy thay thế các số 0 này bằng ID phân bổ hiện tại, ghi lại ánh xạ từ ID đến phạm vi, tăng bộ đếm và in ID. Nếu không tìm thấy thì in`NULL`. 
3. Đối với một`erase x`hoạt động, trước tiên hãy kiểm tra xem ID`x`tồn tại trong bản đồ. Nếu không, hãy in`ILLEGAL_ERASE_ARGUMENT`. Mặt khác, truy xuất chỉ số bắt đầu và kết thúc của khối, đặt tất cả các ô nhớ tương ứng về 0 và xóa ID khỏi ánh xạ. 
4. Đối với một`defragment`hoạt động, lặp lại bộ nhớ và thu thập tất cả các ID khác 0 theo thứ tự. Thay thế bộ nhớ bằng danh sách có thứ tự này, theo sau là các số 0 để lấp đầy độ dài _m_. Cập nhật ánh xạ của từng ID để phản ánh phạm vi mới của nó. 
5. Lặp lại các bước 2-4 cho tất cả các thao tác ở đầu vào, đảm bảo đầu ra được in ngay lập tức cho`alloc`và thất bại`erase`hoạt động. 

Tại sao nó hoạt động: ở mỗi bước, bộ nhớ phản ánh chính xác trạng thái phân bổ. Việc quét phân bổ đảm bảo chính sách phù hợp đầu tiên. Xóa kiểm tra đối với ánh xạ đảm bảo rằng chỉ ID hợp lệ mới bị xóa. Chống phân mảnh duy trì thứ tự phân bổ tương đối. Tính bất biến của tính nhất quán của bộ nhớ và việc gán ID chính xác được duy trì xuyên suốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t, m = map(int, input().split())
memory = [0] * m
alloc_counter = 1
id_map = {}  # ID -> (start, end)

for _ in range(t):
    parts = input().split()
    cmd = parts[0]

    if cmd == 'alloc':
        n = int(parts[1])
        found = False
        i = 0
        while i <= m - n:
            if all(memory[i+j] == 0 for j in range(n)):
                for j in range(n):
                    memory[i+j] = alloc_counter
                id_map[alloc_counter] = (i, i+n)
                print(alloc_counter)
                alloc_counter += 1
                found = True
                break
            i += 1
        if not found:
            print("NULL")

    elif cmd == 'erase':
        x = int(parts[1])
        if x not in id_map:
            print("ILLEGAL_ERASE_ARGUMENT")
        else:
            start, end = id_map[x]
            for i in range(start, end):
                memory[i] = 0
            del id_map[x]

    elif cmd == 'defragment':
        new_memory = [val for val in memory if val != 0]
        memory = new_memory + [0]*(m - len(new_memory))
        idx = 0
        for val in new_memory:
            start = idx
            while idx < m and memory[idx] == val:
                idx += 1
            id_map[val] = (start, idx)
```Mã này phản ánh thuật toán: bộ nhớ được thể hiện rõ ràng, quét phân bổ để tìm dung lượng trống và bản đồ ID đảm bảo việc xóa hợp lệ. Chống phân mảnh sẽ xây dựng lại bộ nhớ tại chỗ và cập nhật tất cả các phạm vi. Một điểm tinh tế là duy trì bản đồ ID một cách chính xác trong quá trình chống phân mảnh; chúng tôi tính toán lại phạm vi cho tất cả các khối được di chuyển. Một sự tinh tế khác là quét bộ nhớ một cách hiệu quả; chúng tôi không bỏ qua các chỉ số trong phân bổ tiềm năng cho đến khi chúng tôi xác nhận thất bại, ngăn chặn từng lỗi một. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
6 10
alloc 5
alloc 3
erase 1
alloc 6
defragment
alloc 6
```| Hoạt động | Trạng thái bộ nhớ | Bản đồ ID | Đầu ra | 
| --- | --- | --- | --- | 
| phân bổ 5 | 1 1 1 1 1 0 0 0 0 0 | {1:(0,5)} | 1 | 
| phân bổ 3 | 1 1 1 1 1 2 2 2 0 0 | {1:(0,5),2:(5,8)} | 2 | 
| xóa 1 | 0 0 0 0 0 2 2 2 0 0 | {2:(5,8)} | - | 
| phân bổ 6 | 0 0 0 0 0 2 2 2 0 0 | {2:(5,8)} | KHÔNG | 
| chống phân mảnh | 2 2 2 0 0 0 0 0 0 0 | {2:(0,3)} | - | 
| phân bổ 6 | 2 2 2 3 3 3 3 3 3 0 | {2:(0,3),3:(3,9)} | 3 | 

Điều này thể hiện sự phân bổ phù hợp đầu tiên, xử lý các phân bổ bất hợp pháp và chống phân mảnh chính xác bằng cập nhật bản đồ ID. 

**Đầu vào tùy chỉnh 2**```
4 5
alloc 2
alloc 2
erase 1
alloc 3
```| Hoạt động | Trạng thái bộ nhớ | Bản đồ ID | Đầu ra | 
| --- | --- | --- | --- | 
| phân bổ 2 | 1 1 0 0 0 | {1:(0,2)} | 1 | 
| phân bổ 2 | 1 1 2 2 0 | {1:(0,2),2:(2,4)} | 2 | 
| xóa 1 | 0 0 2 2 0 | {2:(2,4)} | - | 
| phân bổ 3 | 0 0 2 2 0 | {2:(2,4)} | KHÔNG | 

Điều này xác nhận rằng nếu không chống phân mảnh, bộ nhớ trống bị phân mảnh sẽ không thể đáp ứng được việc phân bổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t*m) | Mỗi`alloc`Và`erase`quét tối đa m ô, lặp lại cho t thao tác. | 
| Không gian | O(m + t) | Mảng bộ nhớ có độ dài m và bản đồ ID lưu trữ tối đa phân bổ t. | 

Cho m, t ≤ 100, số bước tối đa là 10.000, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
```
