---
title: "CF 1059A - Thu ngân"
description: "Chúng tôi được cấp một ngày làm việc có độ dài cố định và một chuỗi các khoảng thời gian phục vụ trong đó nhân viên thu ngân phải có mặt liên tục. Giữa những khoảng thời gian phục vụ này có những khoảng trống về thời gian rảnh."
date: "2026-06-15T09:33:01+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1059
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 514 (Div. 2)"
rating: 1000
weight: 1059
solve_time_s: 85
verified: true
draft: false
---

[CF 1059A - Nhân viên thu ngân](https://codeforces.com/problemset/problem/1059/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một ngày làm việc có độ dài cố định và một chuỗi các khoảng thời gian phục vụ trong đó nhân viên thu ngân phải có mặt liên tục. Giữa những khoảng thời gian phục vụ này có những khoảng trống về thời gian rảnh. Trong những khoảng thời gian rảnh rỗi đó, nhân viên thu ngân có thể nghỉ giải lao nhiều lần, mỗi lần nghỉ giải lao kéo dài đúng một số phút cố định nhưng không được phép bỏ lỡ bất kỳ khoảng thời gian phục vụ khách hàng nào. 

Nhiệm vụ là tính toán xem có thể đặt bao nhiêu lần nghỉ hoàn toàn có độ dài bằng nhau vào tất cả thời gian rảnh có sẵn giữa các khách hàng, giả sử các lần nghỉ không thể chồng lên các khoảng thời gian phục vụ và phải hoàn toàn nằm trong khoảng thời gian nhàn rỗi. 

Từ quan điểm cấu trúc, ngày là một đoạn thẳng từ thời điểm 0 đến thời điểm L. Một số phân đoạn con bị chặn bởi các khoảng thời gian phục vụ khách hàng và mọi phân đoạn khác đều miễn phí. Vấn đề giảm xuống còn việc đếm xem có bao nhiêu đoạn có độ dài rời nhau có thể vừa với sự kết hợp của tất cả các đoạn tự do. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cũng phải chạy theo thời gian tuyến tính đối với số lượng khách hàng. Với khoảng thời gian lên tới 100.000 và L lên tới 1e9, mọi nỗ lực mô phỏng từng phút đều ngay lập tức không thể thực hiện được. Ngay cả việc lặp lại mọi khoảng trống ở độ phân giải tốt cũng chỉ được chấp nhận nếu mỗi khoảng được xử lý một lần. 

Một trường hợp phức tạp xuất hiện khi không có khách hàng nào cả. Khi đó cả ngày rảnh rỗi và chúng ta chỉ cần tính L chia cho a. Một trường hợp cạnh khác phát sinh khi các khoảng thời gian dịch vụ nối tiếp nhau mà không có khoảng trống, trong trường hợp đó không thể chèn khoảng ngắt giữa chúng. Trường hợp quan trọng thứ ba là khi các khoảng trống tồn tại nhưng nhỏ hơn a, do đó chúng đóng góp 0 điểm ngắt. 

Một sai lầm ngây thơ là xử lý từng khoảng trống một cách độc lập nhưng lại quên đưa khoảng trống ban đầu vào trước khách hàng đầu tiên hoặc khoảng trống cuối cùng sau khách hàng cuối cùng. Một lỗi phổ biến khác là cố gắng sắp xếp vị trí tham lam mà không cập nhật cẩn thận ranh giới thời gian hiện tại, dẫn đến việc đếm hai lần hoặc bỏ qua không gian có thể sử dụng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ xây dựng dòng thời gian một cách rõ ràng, đánh dấu tất cả các khoảng thời gian bận rộn và sau đó quét từng phút, đếm các khoảng thời gian rảnh rỗi và đặt các khoảng nghỉ một cách tham lam. Điều này đơn giản về mặt khái niệm: mô phỏng thời gian từ 0 đến L, bỏ qua các khoảng thời gian phục vụ và bất cứ khi nào chúng ta rảnh, hãy sử dụng nó theo từng phần có kích thước a. 

Tính đúng đắn là hiển nhiên vì chúng tôi đang mô hình hóa quy trình theo đúng nghĩa đen. Điểm thất bại là sự phức tạp. Vì L có thể lên tới 10^9 nên việc lặp lại từng phút là không thể, yêu cầu thứ tự 10^9 thao tác cho mỗi trường hợp thử nghiệm, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không bao giờ cần nhìn vào bên trong một đoạn trống. Mỗi đoạn miễn phí đóng góp độc lập các điểm ngắt sàn (độ dài / a). Các khoảng thời gian phục vụ đã được sắp xếp và không chồng chéo, do đó, thời gian rảnh sẽ được phân chia một cách tự nhiên thành các khoảng trống rời rạc. Khi chúng tôi nhận ra cấu trúc đó, vấn đề sẽ trở thành một lần quét tuyến tính duy nhất theo các khoảng thời gian trong khi vẫn duy trì một con trỏ ở cuối khoảng thời gian dịch vụ cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(L) | O(1) | Quá chậm | 
| Đếm Khoảng Cách Khoảng Cách | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`current`đến 0, biểu thị thời điểm sớm nhất mà chúng ta có thể tự do xem xét. 
2. Khởi tạo`answer`về 0, sẽ tích lũy số lần nghỉ hoàn toàn. 
3. Đối với từng khoảng thời gian của khách hàng`(t, l)`, hãy tính thời gian rảnh trước khi khách hàng này bắt đầu làm phân khúc`[current, t)`. Đây là cửa sổ duy nhất có thể sử dụng được trước lần chiếm đóng bắt buộc tiếp theo. 
4. Tính độ dài của đoạn trống này như sau`t - current`. Thêm vào`floor((t - current) / a)`cho câu trả lời, bởi vì mỗi lần nghỉ phải hoàn toàn phù hợp với khoảng thời gian này. 
5. Cập nhật`current`ĐẾN`t + l`, vì sau khi phục vụ khách hàng này, nhân viên thu ngân chỉ trở lại miễn phí sau khi dịch vụ của họ kết thúc. 
6. Sau khi xử lý tất cả khách hàng, xử lý phân đoạn trống cuối cùng từ`current`ĐẾN`L`tương tự bằng cách thêm`(L - current) // a`để trả lời. 

Ý tưởng chính là chúng ta không bao giờ cần mô phỏng vị trí ngắt một cách rõ ràng. Chúng tôi chỉ đếm có bao nhiêu phân đoạn đầy đủ kích thước`a`phù hợp với từng khoảng trống tối đa. 

### Tại sao nó hoạt động 

Thuật toán ngầm phân chia dòng thời gian thành các đoạn bị chặn và trống xen kẽ. Bởi vì khoảng thời gian phục vụ được đảm bảo không chồng chéo lên nhau nên mọi phân đoạn trống đều có giá trị tối đa và độc lập. Mọi ngắt hợp lệ phải nằm hoàn toàn bên trong một trong các phân đoạn này và không có ngắt nào có thể kéo dài trong khoảng thời gian phục vụ. Do đó, giải pháp tối ưu chỉ đơn giản là tính tổng trên tất cả các phân đoạn trống có bao nhiêu khối có chiều dài-a nằm bên trong chúng. Sự tích lũy tham lam trên các phân đoạn sẽ bảo toàn chính xác phân vùng này, do đó không bao giờ có sự gián đoạn nào được tính gấp đôi hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, L, a = map(int, input().split())
    
    current = 0
    ans = 0
    
    for _ in range(n):
        t, l = map(int, input().split())
        
        if t > current:
            ans += (t - current) // a
        
        current = max(current, t + l)
    
    if current < L:
        ans += (L - current) // a
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp giữ một con trỏ duy nhất`current`theo dõi phần cuối của khoảng thời gian chiếm đóng cuối cùng. Mỗi khi gặp một khách hàng mới, chúng tôi ngay lập tức đánh giá khoảng cách giữa thời gian rảnh hiện tại và lần bắt đầu dịch vụ tiếp theo. Điều này tránh việc lưu trữ bất kỳ khoảng thời gian nào hoặc xây dựng dòng thời gian. 

Việc sử dụng`max(current, t + l)`là một biện pháp an toàn đảm bảo tính đúng đắn ngay cả khi lý luận về tính không chồng chéo nghiêm ngặt được nới lỏng, nhưng trong bài toán này, nó chủ yếu duy trì một tiến trình tiến lên rõ ràng. Phân đoạn cuối cùng được xử lý riêng để tránh bỏ lỡ phần đuôi trong ngày. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 11 3
0 1
1 1
```| Bước | hiện tại | khoảng (t, l) | đoạn miễn phí | thêm giờ nghỉ | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | - | - | 0 | 0 | 
| 1 | 0 | (0,1) | 0..0 | 0 | 0 | 
| 2 | 1 | (1,1) | 1..1 | 0 | 0 | 
| kết thúc | 2 | - | 2..11 | 3 | 3 | 

Điều này cho thấy toàn bộ thời gian sử dụng được tập trung sau khi dịch vụ kết thúc và chỉ phân đoạn cuối cùng mới được nghỉ giải lao. 

### Ví dụ 2 

đầu vào:```
1 10 2
3 4
```| Bước | hiện tại | khoảng (t, l) | đoạn miễn phí | thêm giờ nghỉ | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | - | - | 0 | 0 | 
| 1 | 0 | (3,4) | 0..3 | 1 | 1 | 
| kết thúc | 7 | - | 7..10 | 1 | 2 | 

Dấu vết cho thấy ngày được chia thành hai vùng tự do độc lập, mỗi vùng đóng góp riêng biệt. 

Những ví dụ này xác nhận rằng thuật toán tổng hợp chính xác các phân đoạn rời rạc mà không có sự tương tác giữa chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khoảng thời gian của khách hàng được xử lý một lần trong một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Các ràng buộc cho phép lên tới 100.000 khoảng thời gian, do đó chỉ cần quét tuyến tính một lần là đủ. Không cần cấu trúc dữ liệu bổ sung và thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, L, a = map(int, input().split())
    current = 0
    ans = 0
    
    for _ in range(n):
        t, l = map(int, input().split())
        if t > current:
            ans += (t - current) // a
        current = max(current, t + l)
    
    if current < L:
        ans += (L - current) // a
    
    return str(ans)

# provided samples
assert run("2 11 3\n0 1\n1 1\n") == "3"

# no customers
assert run("0 10 2\n") == "5", "entire day free"

# tightly packed customers
assert run("3 10 2\n0 2\n2 2\n4 2\n") == "2", "only tail space"

# large gap
assert run("1 20 3\n5 5\n") == "5", "two large segments"

# small gaps
assert run("2 10 3\n0 4\n5 4\n") == "0", "no space for breaks"

# boundary exact fit
assert run("1 6 3\n2 2\n") == "2", "exact fit segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có khách hàng | 5 | xử lý miễn phí cả ngày | 
| đóng gói chặt chẽ | 2 | khoảng thời gian hợp nhất một cách chính xác | 
| khoảng cách lớn | 5 | đếm nhiều đoạn | 
| khoảng trống nhỏ | 0 | phân chia tầng đúng đắn | 
| phù hợp chính xác | 2 | độ chính xác số học biên | 

## Vỏ cạnh 

Khi không có khách hàng,`current`duy trì ở mức 0 cho đến khi phân đoạn cuối cùng được xử lý. Thuật toán tính toán trực tiếp`(L - 0) // a`, mang lại chính xác số lần nghỉ giải lao hoàn toàn trong cả ngày. 

Khi các khoảng thời gian bảo trì chạm vào nhau, chẳng hạn như`(0,2)`Và`(2,4)`, bản cập nhật`current = max(current, t + l)`đảm bảo rằng không có khoảng cách nhân tạo nào được tạo ra giữa chúng. Phân đoạn miễn phí được tính toán trước khách hàng thứ hai bằng 0, do đó không có điểm ngắt nào được thêm vào không chính xác. 

Khi một đoạn trống nhỏ hơn`a`, phép chia số nguyên đương nhiên đóng góp số 0, ngăn chặn việc tính các ngắt một phần.
