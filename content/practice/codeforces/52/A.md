---
title: "CF 52A - dãy 123"
description: "Chúng ta được cung cấp một danh sách các số nguyên trong đó mỗi số là 1, 2 hoặc 3. Mục tiêu là biến đổi chuỗi sao cho tất cả các số đều giống nhau và chúng ta muốn thực hiện điều này bằng cách sử dụng số thay thế nhỏ nhất có thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 52
codeforces_index: "A"
codeforces_contest_name: "Codeforces Testing Round 1"
rating: 900
weight: 52
solve_time_s: 98
verified: true
draft: false
---

[CF 52A - 123-chuỗi](https://codeforces.com/problemset/problem/52/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên trong đó mỗi số là 1, 2 hoặc 3. Mục tiêu là biến đổi chuỗi sao cho tất cả các số đều giống nhau và chúng ta muốn thực hiện điều này bằng cách sử dụng số thay thế nhỏ nhất có thể. Mỗi lần thay thế bao gồm việc thay đổi một số thành bất kỳ giá trị nào trong hai giá trị còn lại. Đầu vào bắt đầu bằng một số nguyên`n`chỉ định độ dài của chuỗi, theo sau là`n`số nguyên. Đầu ra là một số nguyên duy nhất biểu thị số lượng thay đổi tối thiểu cần thiết. 

Nhìn vào những hạn chế,`n`có thể lớn tới một triệu. Điều này ngay lập tức loại trừ các thuật toán lặp lại tất cả các chuỗi thay thế có thể có hoặc thử mọi kết hợp các phép biến đổi vì độ phức tạp về thời gian của chúng sẽ bùng nổ. Cách tiếp cận theo thời gian tuyến tính được chấp nhận ở đây vì`n`lên tới 10^6 và các thao tác theo thứ tự từ 10^6 đến 10^7 sẽ chạy thoải mái trong giới hạn thời gian 2 giây. 

Các trường hợp đặc biệt cần xem xét bao gồm một chuỗi trong đó tất cả các số đều giống nhau, một chuỗi trong đó mỗi số xuất hiện chính xác với số lần giống nhau và các chuỗi chỉ có một phần tử. Ví dụ, nếu trình tự là`[2]`, không cần thay thế và câu trả lời là`0`. Nếu trình tự là`[1, 2, 3]`, thì lựa chọn tốt nhất là đặt tất cả các số 1, 2 hoặc 3, yêu cầu hai thay đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bạn có thể thử làm cho tất cả các số bằng 1, sau đó tất cả đều bằng 2 và cuối cùng tất cả đều bằng 3, đếm số lượng thay đổi cần thiết trong mỗi trường hợp, sau đó lấy giá trị tối thiểu. Phương pháp này đúng vì một trong ba giá trị mục tiêu này nhất thiết sẽ tạo ra số lượng thay thế tối thiểu. Chi phí cho phương pháp này tỷ lệ thuận với`3 * n`vì với mỗi số mục tiêu, chúng ta phải quét toàn bộ mảng để đếm những số không khớp. Điều này có thể chấp nhận được trong bài toán này vì`3 * 10^6`các hoạt động có thể quản lý được, nhưng trong cài đặt thời gian chặt chẽ hơn hoặc với phạm vi số lớn hơn, phương pháp này sẽ trở nên không hiệu quả. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa vấn đề hơn nữa là nhận ra rằng chúng ta không cần phải mô phỏng việc thay đổi trình tự. Điều quan trọng là mỗi số xuất hiện bao nhiêu lần. Nếu chúng ta biết số lượng 1, 2 và 3 thì số lần thay thế tối thiểu là tổng chiều dài trừ đi số lượng tối đa. Đó là bởi vì lựa chọn tối ưu là chuyển đổi tất cả các phần tử thành số đã xảy ra thường xuyên nhất. Điều này làm giảm vấn đề thành một vấn đề đếm đơn giản, có thể được giải quyết trong một lần truyền mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3 * n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba bộ đếm, một bộ đếm cho mỗi số:`count1`,`count2`, Và`count3`. Mỗi người sẽ theo dõi số lần 1, 2 và 3 xuất hiện trong chuỗi. Đây là thông tin tối thiểu chúng ta cần để quyết định chiến lược thay thế tối ưu. 
2. Lặp lại từng số trong dãy. Nếu số là 1 thì tăng`count1`. Nếu là 2 thì tăng`count2`. Nếu không thì tăng`count3`. Việc đếm như thế này đảm bảo chúng ta có bản đồ tần số hoàn chỉnh trong một lần tuyến tính. 
3. Xác định tần số lớn nhất trong ba lần đếm. Đây là số chúng ta nên chuyển đổi phần còn lại của chuỗi thành. Cho phép`max_count`là lớn nhất của`count1`,`count2`, Và`count3`. 
4. Số lần thay thế tối thiểu cần thiết là tổng chiều dài của chuỗi trừ đi số lượng tối đa này,`n - max_count`. Điều này hoạt động vì các phần tử đếm tối đa đã chính xác và các phần tử còn lại phải được thay đổi để khớp. 

Tại sao nó hoạt động: Tại mỗi thời điểm, chúng tôi duy trì số lần xuất hiện của từng giá trị có thể. Vì chúng ta có thể thay đổi bất kỳ số nào thành bất kỳ số nào khác nên chiến lược tối ưu là luôn chọn giá trị xảy ra thường xuyên nhất và chuyển đổi phần còn lại. Không có chiến lược nào khác có thể yêu cầu ít thay đổi hơn vì bất kỳ sai lệch nào so với số thường xuyên nhất nhất thiết sẽ yêu cầu thay đổi nhiều yếu tố hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

count1 = count2 = count3 = 0
for num in a:
    if num == 1:
        count1 += 1
    elif num == 2:
        count2 += 1
    else:
        count3 += 1

max_count = max(count1, count2, count3)
print(n - max_count)
```Mã bắt đầu bằng cách đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`cho kích thước đầu vào lớn. Sau đó, nó khởi tạo bộ đếm cho từng giá trị có thể có trong chuỗi. Trong quá trình lặp, mỗi số sẽ tăng bộ đếm thích hợp, đảm bảo không xảy ra lỗi sai sót nào. Sau khi đếm, chúng tôi tính số lượng tối đa và trừ nó khỏi`n`để có được số lượng thay thế tối thiểu. Thuật toán tránh các cấu trúc dữ liệu không cần thiết, đảm bảo không gian O(1). 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
9
1 3 2 2 2 1 1 2 3
```| số | đếm1 | đếm2 | đếm3 | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 3 | 1 | 0 | 1 | 
| 2 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 1 | 
| 2 | 1 | 3 | 1 | 
| 1 | 2 | 3 | 1 | 
| 1 | 3 | 3 | 1 | 
| 2 | 3 | 4 | 1 | 
| 3 | 3 | 4 | 2 |`max_count = 4`,`n - max_count = 9 - 4 = 5`. Đầu ra đúng là 5. 

### Tùy chỉnh đầu vào 2```
5
2 2 2 2 2
```| số | đếm1 | đếm2 | đếm3 | 
| --- | --- | --- | --- | 
| 2 | 0 | 1 | 0 | 
| 2 | 0 | 2 | 0 | 
| 2 | 0 | 3 | 0 | 
| 2 | 0 | 4 | 0 | 
| 2 | 0 | 5 | 0 |`max_count = 5`,`n - max_count = 5 - 5 = 0`. Đầu ra đúng là 0. 

Những dấu vết này xác nhận phương pháp đếm xác định chính xác số lượng thay thế tối ưu, xử lý các chuỗi đã đồng nhất và chia tỷ lệ thành các chuỗi lớn hơn một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một tuyến tính đi qua chuỗi để đếm số lần xuất hiện | 
| Không gian | O(1) | Chỉ có ba bộ đếm được sử dụng bất kể kích thước chuỗi | 

Thuật toán phù hợp tốt trong các ràng buộc. Vì`n = 10^6`, một lần tuyến tính là khả thi và chỉ có ba số nguyên được lưu trữ, do đó việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    count1 = count2 = count3 = 0
    for num in a:
        if num == 1:
            count1 += 1
        elif num == 2:
            count2 += 1
        else:
            count3 += 1
    return str(n - max(count1, count2, count3))

# Provided sample
assert run("9\n1 3 2 2 2 1 1 2 3\n") == "5", "sample 1"

# Custom cases
assert run("1\n1\n") == "0", "single element"
assert run("5\n2 2 2 2 2\n") == "0", "all equal"
assert run("3\n1 2 3\n") == "2", "all different"
assert run("6\n1 1 2 2 3 3\n") == "4", "equal counts"
assert run("10\n3 3 3 3 3 3 3 3 3 3\n") == "0", "all 3s"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 0 | Chuỗi phần tử đơn | 
| 5\n2 2 2 2 2 | 0 | Trình tự đã thống nhất | 
| 3\n1 2 3 | 2 | Chuỗi có tất cả các giá trị riêng biệt | 
| 6\n1 1 2 2 3 3 | 4 | Trình tự có số lượng bằng nhau | 
| 10\n3 3 3 3 3 3 3 3 3 3 | 0 | Chuỗi có tất cả các giá trị lặp lại có độ dài tối đa | 

## Vỏ cạnh 

Đối với một chuỗi có một phần tử duy nhất, chẳng hạn như`[1]`, thuật toán tính`count1 = 1`và những cái khác bằng không. Số lượng tối đa là 1
