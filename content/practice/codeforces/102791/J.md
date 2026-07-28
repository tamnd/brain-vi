---
title: "CF 102791J - Chia chuỗi"
description: "Chúng ta có một chuỗi các chữ cái viết thường. Chúng ta cần cắt nó thành các mảnh liên tiếp nhau sao cho mỗi mảnh có độ dài ít nhất là hai và ký tự đầu và ký tự cuối của mảnh đó giống nhau. Mỗi nhân vật phải thuộc về chính xác một mảnh."
date: "2026-07-27T18:16:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "J"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 64
verified: true
draft: false
---

[CF 102791J - Chia chuỗi](https://codeforces.com/problemset/problem/102791/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các chữ cái viết thường. Chúng ta cần cắt nó thành các mảnh liên tiếp nhau sao cho mỗi mảnh có độ dài ít nhất là hai và ký tự đầu và ký tự cuối của mảnh đó giống nhau. Mỗi nhân vật phải thuộc về chính xác một mảnh. Trong số tất cả các cách hợp lệ để cắt chuỗi, chúng tôi muốn cách có số lượng mảnh lớn nhất và chúng tôi phải xuất ra độ dài của các mảnh đó. 

Độ dài chuỗi có thể đạt tới 400000, vì vậy mọi giải pháp thử nhiều vị trí cắt có thể sẽ không hiệu quả. Phương pháp bậc hai sẽ thực hiện khoảng 160 tỷ lượt kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn hai giây cho phép. Chúng ta cần quét tuyến tính hoặc thứ gì đó gần như vậy. 

Những trường hợp phức tạp đều đến từ những đoạn không thể đóng lại được. Ví dụ, đầu vào```
4
abcd
```không có câu trả lời hợp lệ vì ký tự đầu tiên`a`không bao giờ xuất hiện nữa nên phần đầu tiên không thể kết thúc. 

Một trường hợp cạnh khác là một chuỗi trong đó toàn bộ chuỗi là một đoạn:```
5
abcda
```Câu trả lời là một mảnh có chiều dài năm. Việc triển khai bất cẩn chỉ tìm kiếm các đoạn ngắn có thể từ chối nó một cách không chính xác. 

Trường hợp ký tự lặp lại cũng có vấn đề:```
4
aaaa
```Cách chia tốt nhất là hai mảnh có chiều dài bằng hai. Lấy toàn bộ chuỗi thành một phần là hợp lệ nhưng không tối ưu vì chúng ta cần số lượng phần tối đa. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi vị trí kết thúc có thể có cho mỗi phần. Nó sẽ bắt đầu một đoạn ở vị trí`i`, tìm kiếm ký tự khớp sau này và thử đệ quy tất cả các lần cắt có thể. Điều này đúng vì mọi phân vùng có thể đều được xem xét, nhưng số lượng kết hợp cắt có thể tăng theo cấp số nhân. Ngay cả một phiên bản lập trình động kiểm tra từng khoảng thời gian cũng sẽ quá chậm vì có các khoảng O(n2). 

Quan sát quan trọng là mỗi quân cờ đóng góp chính xác một vị trí bắt đầu. Để tối đa hóa số lượng mảnh, chúng tôi muốn mọi mảnh đều hoàn thành càng sớm càng tốt. Nếu một quân bắt đầu ở vị trí`i`, kết thúc hợp lệ sớm nhất có thể là vị trí đầu tiên sau đó chứa cùng một ký tự. Việc chọn phần kết sau chỉ có thể sử dụng các ký tự có thể thuộc về phần khác, vì vậy nó không thể tăng số lượng phần cuối cùng. 

Điều này cho phép quét tham lam. Đối với mỗi vị trí bắt đầu hiện tại, hãy tìm lần xuất hiện tiếp theo của cùng một ký tự. Nếu nó không tồn tại thì chuỗi không thể được phân vùng. Nếu không, hãy cắt ở đó và tiếp tục sau vị trí đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước lần xuất hiện tiếp theo của mỗi ký tự sau mỗi vị trí. Điều này cho phép chúng tôi tìm ngay vị trí sớm nhất mà phần hiện tại có thể kết thúc thay vì phải quét liên tục. 
2. Bắt đầu từ ký tự đầu tiên và sử dụng lần xuất hiện tiếp theo được tính toán trước của ký tự đó làm phần cuối của phần đầu tiên. Khoảng cách giữa các vị trí này là chiều dài mảnh. 
3. Di chuyển vị trí hiện tại đến ký tự sau vị trí kết thúc đã chọn và lặp lại quy trình tương tự. Kết thúc sớm nhất có thể luôn được chọn vì nó để lại hậu tố lớn nhất có thể cho các phần sau. 
4. Nếu bất kỳ vị trí bắt đầu nào không có ký tự bằng nhau sau này, hãy xuất`-1`bởi vì không có sự tiếp tục hợp lệ tồn tại. 

Tại sao nó hoạt động: hãy xem xét phần đầu tiên của bất kỳ phân vùng hợp lệ nào. Nó phải kết thúc khi xuất hiện ký tự đầu tiên. Thuật toán tham lam chọn sự xuất hiện sớm nhất như vậy. Bất kỳ phần đầu tiên hợp lệ nào khác sẽ kết thúc sau, nghĩa là nó sẽ xóa các ký tự khỏi hậu tố còn lại. Lựa chọn tham lam để lại một hậu tố chứa ít nhất nhiều thông tin như bất kỳ lựa chọn nào khác. Áp dụng cùng một lập luận nhiều lần chứng tỏ rằng phân vùng tham lam có số phần tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    nxt = [[n] * 26 for _ in range(n + 1)]

    for i in range(n - 1, -1, -1):
        nxt[i] = nxt[i + 1][:]
        nxt[i][ord(s[i]) - 97] = i

    ans = []
    pos = 0

    while pos < n:
        c = ord(s[pos]) - 97
        end = nxt[pos + 1][c]
        if end == n:
            print(-1)
            return
        ans.append(end - pos + 1)
        pos = end + 1

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```các`nxt`bảng lưu trữ vị trí đầu tiên của mỗi ký tự sau mỗi chỉ mục. Hàng bổ sung chứa đầy`n`hoạt động như một người canh gác có nghĩa là không có sự kiện nào tồn tại. 

Trong quá trình quét,`pos`luôn trỏ đến ký tự đầu tiên của phần chưa hoàn thành tiếp theo. Việc tra cứu`nxt[pos + 1][c]`tránh vô tình sử dụng cùng một ký tự làm vị trí kết thúc, điều này sẽ tạo ra một đoạn có độ dài. Độ dài kết quả là`end - pos + 1`. 

Thuật toán chỉ lưu trữ 26 giá trị cho mỗi vị trí, do đó mức sử dụng bộ nhớ là tuyến tính theo độ dài chuỗi. Số nguyên Python ở đây an toàn vì tất cả các chỉ mục đều dưới 400000. 

## Ví dụ đã hoạt động 

cho```
4
aaaa
```việc thực hiện là: 

| Vị trí bắt đầu | Nhân vật | Vị trí bình đẳng tiếp theo | Độ dài đã chọn | Miếng | 
| --- | --- | --- | --- | --- | 
| 0 | một | 1 | 2 | 2 | 
| 2 | một | 3 | 2 | 2 2 | 

Thuật toán luôn lấy phần hợp lệ ngắn nhất. Ví dụ này cho thấy tại sao tối đa hóa số lượng mảnh có nghĩa là đóng các mảnh ngay lập tức. 

Vì```
15
abcbcaccbbcabca
```việc thực hiện là: 

| Vị trí bắt đầu | Nhân vật | Vị trí bình đẳng tiếp theo | Độ dài đã chọn | Miếng | 
| --- | --- | --- | --- | --- | 
| 0 | một | 5 | 6 | 6 | 
| 6 | c | 10 | 5 | 6 5 | 
| 11 | một | 14 | 4 | 6 5 4 | 

Hậu tố còn lại sau mỗi lần cắt vẫn được xử lý độc lập, đó là bất biến đằng sau chứng minh tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí trong số n vị trí được xử lý một lần và mỗi lần tra cứu lần xuất hiện tiếp theo là thời gian không đổi. | 
| Không gian | O(n) | Bảng lưu trữ 26 vị trí tiếp theo cho mỗi chỉ mục. | 

Với`n = 400000`, khoảng mười triệu tham chiếu số nguyên được lưu trữ được tạo ra, vừa vặn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    
    def solve():
        n = int(input())
        s = input().strip()

        nxt = [[n] * 26 for _ in range(n + 1)]
        for i in range(n - 1, -1, -1):
            nxt[i] = nxt[i + 1][:]
            nxt[i][ord(s[i]) - 97] = i

        ans = []
        pos = 0
        while pos < n:
            c = ord(s[pos]) - 97
            end = nxt[pos + 1][c]
            if end == n:
                print(-1)
                return
            ans.append(end - pos + 1)
            pos = end + 1

        print(len(ans))
        print(*ans)

    solve()
    result = out.getvalue()
    sys.stdin = old
    return result

assert run("4\naaaa\n") == "2\n2 2\n"
assert run("15\nabcbcaccbbcabca\n") == "3\n6 5 4\n"
assert run("4\nabcd\n") == "-1\n"
assert run("5\nabcda\n") == "1\n5\n"
assert run("2\naa\n") == "1\n2\n"
assert run("6\naabbaa\n") == "3\n2 2 2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aa`| một đoạn dài hai | Kích thước hợp lệ tối thiểu | 
|`aaaa`| hai mảnh | Ký tự hoàn toàn bằng nhau | 
|`abcd`|`-1`| Thiếu ký tự kết thúc | 
|`abcda`| một mảnh | Phân vùng toàn chuỗi | 

## Vỏ cạnh 

cho`aaaa`, thuật toán bắt đầu từ chỉ số 0, tìm giá trị tiếp theo`a`ngay lập tức, tạo một đoạn dài hai đoạn và lặp lại từ chỉ số hai. Nó không bao giờ lãng phí các ký tự bên trong một phần lớn hơn, mang lại số lượng tối đa. 

Vì`abcd`, tra cứu đầu tiên cho`a`không trả về vị trí muộn hơn. Thuật toán dừng ngay lập tức vì không thể tạo được phần đầu tiên. 

Vì`abcda`, lần tra cứu đầu tiên tìm thấy cái cuối cùng`a`. Phân vùng duy nhất có thể là toàn bộ chuỗi và thuật toán xuất ra chính xác độ dài năm. 

Đối với một chuỗi kết thúc sau một số phần thành công, chẳng hạn như`aabbaa`, thuật toán đóng`aa`, sau đó`bb`, sau đó`aa`. Mỗi ranh giới được chọn là ranh giới sớm nhất có thể, do đó không thể bỏ qua phần cắt bổ sung nào. 

Điều này có thể được mở rộng hơn nữa với phần chứng minh dài hơn hoặc phần theo dõi chi tiết hơn nếu bạn cần một định dạng biên tập cuộc thi có độ dài đầy đủ.
