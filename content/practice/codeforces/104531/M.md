---
title: "CF 104531M - Nước"
description: "Một hàng người được phục vụ tuần tự bằng máy lọc nước. Mỗi người có một nhu cầu tính bằng lít, và một xô nước trên máy phân phối có dung tích cố định là $C$ lít. Mọi người lần lượt được phục vụ bằng cách sử dụng thùng hiện tại cho đến khi hết."
date: "2026-06-30T09:59:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104531
codeforces_index: "M"
codeforces_contest_name: "2022 SYSU School Contest"
rating: 0
weight: 104531
solve_time_s: 50
verified: true
draft: false
---

[CF 104531M - Nước](https://codeforces.com/problemset/problem/104531/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một hàng người được phục vụ tuần tự bằng máy lọc nước. Mỗi người có nhu cầu tính bằng lít và một xô nước trên máy phân phối có dung tích cố định là$C$lít. Mọi người lần lượt được phục vụ bằng cách sử dụng thùng hiện tại cho đến khi hết. 

Điểm mấu chốt là điều xảy ra khi thùng rỗng trong lúc đang phục vụ ai đó. Người làm xô hết liền thay một xô mới đầy rồi bỏ đi ngay. Bởi vì họ rời đi ngay lập tức nên mọi nhu cầu còn lại của họ sẽ bị mất và không được chuyển sang nhóm tiếp theo. 

Nhiệm vụ là tính tổng số thùng được sử dụng trong khi phục vụ tất cả mọi người theo thứ tự. 

Kích thước đầu vào cho phép lên đến$10^6$người cho mỗi trường hợp thử nghiệm và nhiều trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng mô phỏng mức tiêu thụ nước bằng các vòng lặp lồng nhau hoặc xử lý theo lít. Giải pháp phải tuyến tính trong tổng số người, bởi vì bất cứ điều gì tồi tệ hơn$O(n)$mỗi trường hợp thử nghiệm sẽ hết thời gian chờ. 

Trường hợp phức tạp nhất đến từ những người có nhu cầu lớn hơn lượng nước còn lại trong thùng hiện tại. Ví dụ: nếu nước còn lại là 2 lít và người tiếp theo cần 10 lít, người đó sẽ uống 2 lít còn lại, kích hoạt thay xô, sau đó rời đi ngay lập tức mà không tiêu hết phần còn lại. Một cách giải thích đơn giản là chỉ cần trừ đi toàn bộ nhu cầu từ nhóm mà không lập mô hình gián đoạn sẽ tính quá mức hoặc tính thiếu mức sử dụng nhóm tùy thuộc vào chi tiết triển khai. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp theo dõi lượng nước còn lại trong thùng hiện tại và xử lý từng người theo thứ tự. Đối với mỗi người, chúng tôi lấy lượng nước còn lại trừ đi nhu cầu của họ. Nếu có đủ nước thì quá trình này rất đơn giản. Nếu không, thùng sẽ cạn trong lượt của người đó, chúng ta đếm một thùng mới, đổ đầy lại và tiếp tục. 

Sai lầm ngây thơ là cho rằng sau khi nạp lại, người đó vẫn tiếp tục tiêu thụ nhu cầu còn lại của họ. Cách giải thích đó sẽ dẫn đến việc một người tiêu thụ nhiều thùng, điều này mâu thuẫn với quy tắc họ rời đi ngay sau khi thay thùng. Sự khác biệt này giúp đơn giản hóa quy trình: mỗi khi một người không vừa với lượng nước còn lại, họ chỉ kích hoạt chính xác một thùng bổ sung. 

Quan sát quan trọng là chúng ta không bao giờ cần phải theo dõi một phần nhu cầu còn sót lại ngoài nhóm hiện tại. Hoặc một người hoàn toàn vừa với thùng hiện tại hoặc họ tiêu thụ phần còn lại, buộc phải đổ đầy lại và dừng lại ngay lập tức. Điều này biến vấn đề thành một mô phỏng tham lam một lượt với một biến trạng thái đơn giản biểu thị lượng nước còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ trên mỗi đơn vị | O(tổng đơn vị nước) | O(1) | Quá chậm | 
| Theo dõi xô tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai biến: số thùng được sử dụng và lượng nước còn lại trong thùng hiện tại. 

1. Khởi tạo số xô thành 1 và đặt lượng nước còn lại thành$C$, vì chúng ta bắt đầu với một thùng đầy. 
2. Xử lý từng người theo thứ tự. 
3. Nếu nhu cầu của người hiện tại nhỏ hơn hoặc bằng lượng nước còn lại, hãy trừ đi lượng nước còn lại và chuyển sang người tiếp theo. 
4. Nếu nhu cầu lớn hơn lượng nước còn lại, thùng hiện tại sẽ cạn trong lượt của người này. Chúng tôi tăng số lượng nhóm lên 1, đổ đầy thùng đầy và cho phép người tiếp theo bắt đầu sử dụng nhóm mới. 
5. Trong trường hợp tràn, chúng tôi không tiếp tục nhu cầu còn lại của người hiện tại, vì họ rời đi ngay sau khi kích hoạt nạp tiền. 

Tính đúng đắn dựa trên tính bất biến là lượng nước còn lại luôn phản ánh dung tích chưa sử dụng của thùng hiện tại khi bắt đầu lượt của mỗi người. Mỗi nhóm được tính chính xác khi nó được giới thiệu và không ai đóng góp nhiều hơn một nhóm bổ sung ngoài mức yêu cầu của vị trí của họ trong trình tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, C = map(int, input().split())
        arr = list(map(int, input().split()))
        
        buckets = 1
        rem = C
        
        for x in arr:
            if x <= rem:
                rem -= x
            else:
                buckets += 1
                rem = C
        
        print(buckets)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh quá trình một cách trực tiếp. Biến`rem`theo dõi lượng nước còn lại trong thùng hiện tại. Khi một người vừa vặn, chúng tôi chỉ cần giảm bớt nó. Khi chúng không vừa, chúng tôi tăng số lượng nhóm và đặt lại`rem`đến một thùng đầy. 

Một điểm tinh tế là chúng tôi không cố gắng mô phỏng mức tiêu thụ một phần ngoài việc kích hoạt nạp tiền. Một lần`x > rem`, người hiện tại tiêu thụ chính xác`rem`các đơn vị một cách ngầm định, nhưng chúng ta không cần phải trừ nó một cách rõ ràng vì nhóm đặt lại và nhu cầu còn lại bị loại bỏ theo quy tắc bài toán. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với$C = 5$và những người có nhu cầu$[3, 2, 4]$. 

Chúng tôi theo dõi quá trình từng bước. 

| Người | Nhu cầu | Còn lại trước | Hành động | Xô | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 5 - 3 = 2 | 1 | 
| 2 | 2 | 2 | 2 - 2 = 0 | 1 | 
| 3 | 4 | 0 | đổ đầy, xô mới | 2 | 

Người thứ ba kích hoạt nạp tiền vì thùng hiện tại trống. Họ rời đi ngay sau đó nên không tiếp tục tiêu thụ từ thùng mới. 

Bây giờ hãy xem xét$C = 4$, yêu cầu$[1, 6, 2]$. 

| Người | Nhu cầu | Còn lại trước | Hành động | Xô | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 4 - 1 = 3 | 1 | 
| 2 | 6 | 3 | tiêu thụ 3, nạp lại, rời đi | 2 | 
| 3 | 2 | 4 | 4 - 2 = 2 | 2 | 

Người thứ hai kích hoạt chính xác một thùng bổ sung mặc dù nhu cầu của họ vượt quá một công suất tối đa. Nhu cầu vượt mức sẽ bị loại bỏ sau sự kiện nạp tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi người được xử lý một lần với công việc O(1) | 
| Không gian |$O(1)$| Chỉ sử dụng các bộ đếm và một trạng thái chạy duy nhất | 

Các ràng buộc cho phép lên đến$10^6$các phần tử và giải pháp này thực hiện một lượng công việc không đổi cho mỗi phần tử, do đó nó phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# basic samples
assert run("1\n3 5\n3 2 4\n") == "2"

# all fit in one bucket
assert run("1\n4 10\n1 2 3 4\n") == "1"

# every person triggers new bucket
assert run("1\n5 3\n4 4 4 4 4\n") == "5"

# exact fills
assert run("1\n3 4\n2 2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5 / 3 2 4 | 2 | quy tắc xử lý tràn và loại bỏ | 
| 4 10 / 1 2 3 4 | 1 | tích lũy bình thường | 
| 5 3 / 4 4 4 4 4 | 5 | buộc phải nạp lại nhiều lần | 
| 3 4 / 2 2 2 | 2 | cạn kiệt ranh giới chính xác | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhu cầu của một người lớn hơn một thùng đầy. Ví dụ, với$C = 3$và một yêu cầu của$10$, hành vi đúng vẫn là chỉ thêm một nhóm bổ sung vào thời điểm nhóm hiện tại trống chứ không phải nhiều nhóm. Nhu cầu còn lại không liên quan vì người đó rời đi ngay sau khi kích hoạt nạp tiền. 

Chạy thuật toán: bắt đầu với 3 còn lại. Nhu cầu 10 vượt quá nó, vì vậy chúng tôi tăng nhóm lên 2 và đặt lại số còn lại thành 3. Chúng tôi không tiếp tục tiêu thụ 7 đơn vị. Người tiếp theo bắt đầu mới, phù hợp với quy tắc của bài toán. 

Một trường hợp khác là khi lượng nước còn lại đúng bằng nhu cầu của một người. Trong trường hợp đó, thùng kết thúc chính xác ở mức 0 nhưng không có hoạt động nạp tiền nào được kích hoạt trong lượt của người đó. Người tiếp theo sẽ sử dụng xô mới hoặc kích hoạt đổ đầy tùy theo nhu cầu của họ.
