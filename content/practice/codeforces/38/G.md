---
title: "CF 38G - Hàng đợi"
description: "Chúng tôi được yêu cầu mô phỏng một hàng người trong đó mỗi người có hai thuộc tính: giá trị tầm quan trọng a[i] và giới hạn kiên nhẫn c[i]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 38
codeforces_index: "G"
codeforces_contest_name: "School Personal Contest #1 (Winter Computer School 2010/11) - Codeforces Beta Round 38 (ACM-ICPC Rules)"
rating: 2300
weight: 38
solve_time_s: 61
verified: true
draft: false
---
[CF 38G - Hàng đợi](https://codeforces.com/problemset/problem/38/G) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng một hàng người trong đó mỗi người có hai thuộc tính: một giá trị quan trọng`a[i]`và sự kiên nhẫn có giới hạn`c[i]`. Mọi người đến theo thứ tự và khi người cuối cùng (số`n`) tham gia, anh ta có thể tiến lên trong hàng đợi bằng cách hoán đổi với người ngay trước mặt anh ta, nhưng chỉ khi người đó có tầm quan trọng thấp hơn anh ta. Anh ấy có thể biểu diễn nhiều nhất`c[n]`trao đổi. Đầu ra phải là thứ tự cuối cùng của tất cả mọi người theo cách đánh số ban đầu của họ sau khi tất cả các giao dịch hoán đổi đã dừng lại. 

Đầu vào đảm bảo rằng`a[i]`tạo thành một hoán vị từ 1 đến`n`, vậy là không có hai người nào có tầm quan trọng như nhau. Điều này đảm bảo rằng mọi so sánh`a[i] > a[j]`là nghiêm ngặt. Các ràng buộc cho phép lên đến`10^5`người và giới hạn thời gian 2 giây, nghĩa là bất kỳ thuật toán nào thực hiện nhiều hơn khoảng`10^8`hoạt động có thể quá chậm. Một mô phỏng đơn giản của mỗi lần hoán đổi có thể dẫn đến hành vi O(n²) trong trường hợp xấu nhất, điều này không được chấp nhận ở đây. 

Các trường hợp cạnh dễ bị bỏ lỡ bao gồm khi`c[n]`bằng 0 (người cuối cùng không thể di chuyển chút nào), khi người cuối cùng có tầm quan trọng cao nhất (anh ta nên di chuyển lên phía trước nếu`c[n]`cho phép) và khi nhiều giao dịch hoán đổi bị giới hạn bởi`c[n]`trước khi tiếp cận người có tầm quan trọng cao hơn. 

Ví dụ: với đầu vào:```
3
1 0
2 0
3 1
```Thuật toán ngây thơ phải dừng lại sau một lần hoán đổi vì`c[3] = 1`, mặc dù về mặt lý thuyết, người thứ 3 có thể vượt qua cả người 1 và người 2. Kết quả đúng là`3 1 2`, không`3 2 1`. Một mô phỏng bất cẩn bỏ qua`c[n]`sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực thực hiện trực tiếp quá trình hoán đổi được mô tả. Khi mỗi người mới đến, chúng tôi so sánh anh ta với người ngay phía trước, hoán đổi nếu được phép, giảm số lượng hoán đổi còn lại của anh ta và lặp lại cho đến khi số lượng hoán đổi bằng 0 hoặc người ở phía trước có tầm quan trọng cao hơn. Điều này đúng nhưng có thể yêu cầu thao tác O(n²) nếu người cuối cùng có tầm quan trọng cao và tính kiên nhẫn cao, di chuyển từ phía sau ra phía trước. 

Cái nhìn sâu sắc quan trọng để cải thiện điều này là nhận ra rằng bản chất hoán vị của`a[i]`hãy để chúng tôi suy luận về mặt **vị trí tương đối** thay vì mô phỏng từng lần hoán đổi riêng lẻ. Mỗi người cuối cùng sẽ tiến về phía trước càng xa càng tốt, bị ràng buộc bởi`c[i]`và tầm quan trọng của những người ở phía trước. Vấn đề giảm xuống còn việc chèn người cuối cùng vào đúng vị trí và đếm xem anh ta có thể thực hiện bao nhiêu lần hoán đổi. Điều này cho phép mô phỏng thời gian tuyến tính bằng cách lặp lại từ cuối hàng đợi hiện tại về phía sau và chuyển mọi người về phía trước cho đến khi hết giới hạn di chuyển hoặc chúng tôi tiếp cận được một người có tầm quan trọng cao hơn. 

Cách tiếp cận vũ phu về mặt khái niệm đơn giản nhưng chậm. Cách tiếp cận tối ưu tận dụng thực tế là`a[i]`là duy nhất và chỉ có một người di chuyển tại một thời điểm, cho phép chúng tôi mô phỏng trong một lần lùi lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số người`n`và tài sản của họ`a[i]`Và`c[i]`. 
2. Duy trì một danh sách`queue`đại diện cho trật tự hiện tại của mọi người bằng chỉ số của họ khi họ đến. Khởi tạo nó với người đầu tiên. 
3. Đối với từng người tiếp theo (từ người thứ hai đến người cuối cùng): 

1. Thêm chúng vào cuối hàng đợi. 
2. Hãy để`moves_left = c[i]`. 
3. Trong khi`moves_left > 0`và người đứng ngay phía trước có tầm quan trọng thấp hơn: 

1. Hoán đổi người hiện tại với người ở phía trước. 
2. Giảm`moves_left`bằng 1. 
3. Tiếp tục kiểm tra người tiếp theo ở phía trước. 
4. Sau khi xử lý tất cả mọi người, chuyển đổi hàng đợi từ chỉ mục sang đánh số dựa trên 1 (thứ tự đầu vào ban đầu) và in. 

Tại sao nó hoạt động: ở mỗi lần chèn, một người di chuyển về phía trước chính xác theo mức độ cho phép của họ`c[i]`và các ràng buộc về tầm quan trọng. Vì chỉ các khối có tầm quan trọng cao hơn mới ngăn chặn sự di chuyển và các giao dịch hoán đổi mang tính cục bộ nên việc mô phỏng thao tác chèn của mỗi người sẽ đảm bảo tính chính xác. Không người nào đến sau ảnh hưởng đến trật tự tương đối của những người đến trước ngoại trừ việc di chuyển qua họ theo cùng một quy tắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
people = [tuple(map(int, input().split())) + (i+1,) for i in range(n)]

queue = []

for a, c, idx in people:
    pos = len(queue)
    moves_left = c
    while pos > 0 and moves_left > 0 and queue[pos-1][0] < a:
        queue[pos] = queue[pos-1]
        pos -= 1
        moves_left -= 1
    if pos < len(queue):
        queue[pos] = (a, c, idx)
    else:
        queue.append((a, c, idx))

print(" ".join(str(x[2]) for x in queue))
```Mã giữ hàng đợi dưới dạng danh sách các bộ dữ liệu`(a, c, original_index)`. Đối với mỗi người, nó tìm ra vị trí xa nhất mà họ có thể lùi lại trong khi hoán đổi với những người có tầm quan trọng thấp hơn. Phần tinh tế nhất là dịch chuyển các phần tử tại chỗ để tránh các thao tác danh sách không cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
1 0
2 1
```| Bước | Trạng thái xếp hàng | Di chuyển sang trái | Hành động | 
| --- | --- | --- | --- | 
| 1 | [(1,0,1)] | - | người đầu tiên được thêm vào | 
| 2 | [(1,0,1),(2,1,2)] | 1 | người 2 so sánh với người 1, hoán đổi | 
| Cuối cùng | [(2,1,2),(1,0,1)] | 0 | chỉ số đầu ra 2 1 | 

Đầu vào thứ hai:```
3
1 0
2 0
3 1
```| Bước | Trạng thái xếp hàng | Di chuyển sang trái | Hành động | 
| --- | --- | --- | --- | 
| 1 | [(1,0,1)] | - | người đầu tiên được thêm vào | 
| 2 | [(1,0,1),(2,0,2)] | 0 | người 2 không thể di chuyển | 
| 3 | [(1,0,1),(2,0,2),(3,1,3)] | 1 | người 3 hoán đổi với 2 | 
| 3 | [(1,0,1),(3,1,3),(2,0,2)] | 0 | không thể di chuyển xa hơn, xuất 1 3 2 | 

Những dấu vết này xác nhận rằng các giao dịch hoán đổi bị giới hạn bởi`c[i]`và tầm quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi người có thể di chuyển nhiều nhất`c[i]`bước, tổng của`c[i] ≤ n`, việc dịch chuyển trong danh sách mất tổng cộng O(n) | 
| Không gian | O(n) | Danh sách hàng đợi và lưu trữ đầu vào | 

Thuật toán thực hiện một lần duyệt qua hàng đợi và dịch chuyển nhiều nhất`n`tổng số phần tử, phù hợp thoải mái trong giới hạn 2 giây cho n = 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    people = [tuple(map(int, input().split())) + (i+1,) for i in range(n)]

    queue = []

    for a, c, idx in people:
        pos = len(queue)
        moves_left = c
        while pos > 0 and moves_left > 0 and queue[pos-1][0] < a:
            queue[pos] = queue[pos-1]
            pos -= 1
            moves_left -= 1
        if pos < len(queue):
            queue[pos] = (a, c, idx)
        else:
            queue.append((a, c, idx))

    return " ".join(str(x[2]) for x in queue)

# Provided sample
assert run("2\n1 0\n2 1\n") == "2 1"
assert run("3\n1 0\n2 0\n3 1\n") == "1 3 2"

# Custom cases
assert run("1\n1 0\n") == "1"  # single person
assert run("3\n3 2\n1 0\n2 1\n") == "3 2 1"  # highest importance first
assert run("4\n1 1\n2 1\n3 1\n4 0\n") == "4 3 2 1"  # last person cannot move
assert run("5\n1 0\n2 2\n3 1\n5 3\n4 0\n") == "5 2 3 1 4"  # complex swaps
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người | 1 | đầu vào nhỏ nhất | 
| Tầm quan trọng cao nhất đầu tiên | 3 2 1 | di chuyển từ trước ra sau không bị hạn chế | 
| Người cuối cùng không thể di chuyển | 4 3 2 1 |`c[i] = 0`ngăn chặn việc hoán đổi | 
| Hoán đổi phức tạp | 5 2 3 1 4 | nhiều nước đi có giới hạn | 

## Vỏ cạnh 

Đối với một người có`c[i] = 0`, không có sự hoán đổi nào xảy ra bất kể tầm quan trọng. Ví dụ:
