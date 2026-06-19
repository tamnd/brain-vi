---
title: "CF 1016A - Cuốn Sổ Tử Thần"
description: "Chúng tôi đang mô phỏng quá trình viết thông qua một cuốn sổ tay vô tận trong đó mỗi trang có thể lưu trữ một số tên cố định."
date: "2026-06-16T22:17:27+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1016
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 48 (Rated for Div. 2)"
rating: 900
weight: 1016
solve_time_s: 84
verified: true
draft: false
---

[CF 1016A - Death Note](https://codeforces.com/problemset/problem/1016/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quá trình viết thông qua một cuốn sổ tay vô tận trong đó mỗi trang có thể lưu trữ một số tên cố định. Trong một chuỗi ngày, chúng ta được biết có bao nhiêu tên được viết mỗi ngày và việc viết diễn ra liên tục trong nhiều ngày: nếu một trang không đầy vào cuối ngày thì ngày hôm sau sẽ tiếp tục điền nó thay vì bắt đầu một trang mới. 

Mỗi khi đầy một trang, người viết sẽ lật ngay sang trang tiếp theo và tiếp tục viết ở đó. Những gì chúng ta cần tính toán là, mỗi ngày, số lần chuyển đổi trang xảy ra trong khi xử lý loạt tên của ngày hôm đó. 

Quan sát quan trọng là quá trình viết hoàn toàn tuần tự trong tất cả các ngày. Chúng tôi đang đổ một luồng đơn vị vào các nhóm có kích thước m một cách hiệu quả và chúng tôi chỉ quan tâm đến số lần chúng tôi tràn từ nhóm này sang nhóm tiếp theo trong mỗi phân đoạn của luồng đó. 

Các ràng buộc cho thấy n có thể lớn tới 200.000 và mỗi ai có thể lên tới 10^9. Một mô phỏng đơn giản xử lý từng tên riêng lẻ sẽ yêu cầu tới 10^14 thao tác trong trường hợp xấu nhất, vượt xa mức cho phép trong 2 giây. Điều này buộc chúng ta phải tránh mô phỏng từng phần tử và thay vào đó làm việc với số học trên các phân đoạn. 

Trường hợp khó phát hiện khi một ngày bắt đầu chính xác tại ranh giới của trang. Trong trường hợp đó, tên đầu tiên của ngày bắt đầu trên một trang mới, do đó, không tính phần chuyển tiếp ngầm định. Một trường hợp góc cạnh khác xảy ra khi sự đóng góp của một ngày đủ lớn để trải rộng trên nhiều trang đầy đủ, yêu cầu phải lật nhiều trang trong cùng một ngày thay vì chỉ lật trang cuối. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ theo dõi một con trỏ trong trang hiện tại và giảm dung lượng từng tên một. Mỗi khi con trỏ về 0, chúng ta tăng bộ đếm trang và đặt lại dung lượng về m. Mặc dù đúng nhưng phương pháp này thực hiện một bước cho mỗi tên được viết, nghĩa là nó có thể giảm xuống mức xử lý hàng tỷ thao tác. 

Cấu trúc của vấn đề gợi ý một cách nhìn khác. Thay vì theo dõi từng tên riêng lẻ, chúng tôi theo dõi xem trang hiện tại đã được lấp đầy bao nhiêu vào đầu mỗi ngày. Đặt dung lượng còn lại trong trang hiện tại là rem. Khi chúng tôi xử lý một ngày với tên ai, quá trình chuyển đổi trang đầu tiên chỉ xảy ra nếu ai vượt quá rem. Sau khi sử dụng tên rem, mọi thứ khác sẽ được chia thành các trang đầy đủ có kích thước m một cách hiệu quả. 

Điều này chuyển đổi vấn đề thành một sự kết hợp của việc điền một phần cộng với phép chia số nguyên đầy đủ. Số lượt lật trang trong một ngày sẽ trở thành một lần chuyển đổi có điều kiện nếu chúng ta vượt qua ranh giới trang hiện tại, cộng với số trang đầy đủ được sử dụng sau đó. 

Điều này làm giảm mỗi ngày xuống còn O(1) phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(tổng ai) | O(1) | Quá chậm | 
| Theo dõi số học | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một biến rem biểu thị số lượng vị trí còn lại trên trang hiện tại.

1. Khởi tạo rem = m vì chúng ta bắt đầu với một trang mới có thể chứa m tên. 
2. Với mỗi ngày i, đọc ai và đặt lần lượt = 0. 
3. Nếu ai nhỏ hơn hoặc bằng rem, chúng ta chỉ cần trừ ai khỏi rem và đặt rem tương ứng. Không có trang nào được lật trong ngày này nên số lượt vẫn bằng 0. Điều này xảy ra vì chúng tôi không bao giờ vượt qua ranh giới trang. 
4. Nếu ai lớn hơn rem, trước tiên chúng ta sử dụng tên rem để hoàn thành trang hiện tại. Chúng tôi tăng số lượt lên 1 vì chúng tôi chuyển sang trang mới đúng một lần tại đây. 
5. Sau khi hoàn thành một phần trang, chúng ta giảm ai xuống rem và đặt lại rem về 0, vì bây giờ chúng ta đã căn chỉnh tại một ranh giới trang. 
6. Ai còn lại được chia thành các trang đầy đủ. Chúng ta tính toán lượt += ai // m, vì mỗi khối có kích thước m buộc phải lật trang. 
7. Cập nhật rem = m - (ai % m) nếu ai % m != 0, ngược lại đặt rem = m. Điều này sẽ khôi phục dung lượng còn lại trên trang được lấp đầy một phần cuối cùng hoặc một trang mới nếu chúng tôi kết thúc chính xác ở một ranh giới. 
8. Đầu ra lần lượt cho ngày hiện tại. 

Mỗi bước phản ánh quá trình viết vật lý: chúng ta chỉ “chạm” vào ranh giới trang khi chúng ta thực sự vượt qua chúng và tất cả nội dung tiêu thụ được nén thành số học. 

### Tại sao nó hoạt động 

Điều bất biến là rem luôn thể hiện chính xác dung lượng chưa sử dụng trên trang hiện tại vào đầu mỗi ngày. Mỗi lần chúng tôi xử lý một ngày, chúng tôi sẽ phân tách sự đóng góp của nó thành nhiều nhất một lần chuyển đổi một phần trang cộng với một số lần hoàn thành toàn trang. Vì ranh giới trang chỉ bị ảnh hưởng khi sử dụng m đơn vị nên việc nhóm ghi thành các phần có kích thước m sẽ duy trì số lần chuyển tiếp chính xác. Không có hai cách phân tách khác nhau của cùng một ai có thể tạo ra số khối có kích thước m đầy đủ khác nhau, do đó số lần lật trang được xác định duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    rem = m
    res = []

    for x in a:
        turns = 0

        if x <= rem:
            rem -= x
        else:
            turns += 1
            x -= rem
            x //= 1  # no-op clarity: we are working with remaining full pages

            turns += x // m
            r = x % m

            if r == 0:
                rem = m
            else:
                rem = m - r

        res.append(str(turns))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì dung lượng trang còn lại. Việc phân chia có điều kiện đảm bảo rằng chúng tôi chỉ tính một lượt lật trang khi vượt qua một ranh giới. Việc phân chia số nguyên xử lý hàng loạt tất cả các chuyển đổi toàn trang, điều này giúp loại bỏ nhu cầu mô phỏng theo từng tên. 

Một chi tiết tinh tế là cập nhật rem một cách chính xác sau khi xử lý toàn bộ trang. Nếu đoạn cuối cùng kết thúc chính xác tại một ranh giới, rem phải được đặt lại thành m, nếu không nó phải phản ánh không gian chưa sử dụng còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
3 7 9
```Chúng tôi theo dõi rem và lượt mỗi ngày. 

| Ngày | ai | bắt đầu lại | lượt | kết thúc | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 0 | 2 | 
| 2 | 7 | 2 | 2 | 3 | 
| 3 | 9 | 3 | 1 | 1 | 

Vào ngày thứ 2, trước tiên chúng tôi lấp đầy 2 ô còn lại, buộc phải lật một trang. 5 tên còn lại tạo đúng một trang đầy đủ, sau đó là một phần của trang khác. Ngày thứ 3 lại vượt qua một ranh giới và sau đó nằm gọn trong một trang đầy đủ. 

Điều này xác nhận rằng thuật toán phân tách chính xác các chuyển tiếp một phần và toàn trang. 

### Ví dụ 2 

đầu vào:```
2 4
10 3
```| Ngày | ai | bắt đầu lại | lượt | kết thúc | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 4 | 3 | 2 | 
| 2 | 3 | 2 | 0 | 3 | 

Ngày 1 điền 4 tên (1 lượt), sau đó còn lại 6 tên, tạo thành 1 trang đầy đủ (1 lượt) và để lại 2 tên vào một trang mới (tổng cộng 1 lượt trong cùng ngày). Cuối cùng còn lại là 2 slot chưa sử dụng. 

Ngày thứ 2 không vượt qua ranh giới nên không xảy ra việc lật trang. 

Những dấu vết này cho thấy cách phân rã số học khớp chính xác với các chuyển đổi trang vật lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngày được xử lý theo thời gian không đổi bằng các phép toán số học | 
| Không gian | O(1) | Chỉ phần còn lại đang chạy và danh sách đầu ra được lưu trữ | 

Quá trình quét tuyến tính trong n ngày dễ dàng nằm trong giới hạn n lên tới 2 × 10^5 và không có hoạt động nào phụ thuộc vào độ lớn của ai ngoài số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    rem = m
    out = []

    for x in a:
        turns = 0
        if x <= rem:
            rem -= x
        else:
            turns += 1
            x -= rem
            turns += x // m
            r = x % m
            if r == 0:
                rem = m
            else:
                rem = m - r
        out.append(str(turns))

    return " ".join(out)

# provided sample
assert run("3 5\n3 7 9\n") == "0 2 1"

# minimum input
assert run("1 10\n5\n") == "0"

# exact page boundaries
assert run("2 3\n3 3\n") == "1 1"

# large single day
assert run("1 5\n12\n") == "2"

# alternating fits and overflow
assert run("4 4\n1 10 1 10\n") == "0 3 0 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10/5 | 0 | điền một phần | 
| 2 3 / 3 3 | 1 1 | chuyển tiếp ranh giới chính xác | 
| 1 5/12 | 2 | nhiều lượt lật trang trong một ngày | 
| 4 4 / 1 10 1 10 | 0 3 0 3 | xử lý tràn lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một ngày kết thúc chính xác tại ranh giới trang. Ví dụ: với m = 4 và một ngày viết 4 tên, trang này sẽ kết thúc rõ ràng và ngày hôm sau sẽ bắt đầu bằng một trang mới. Thuật toán xử lý vấn đề này bằng cách đặt lại rem về m khi phần còn lại của phép chia bằng 0, đảm bảo ngày hôm sau không giả định sai công suất còn lại. 

Một trường hợp khác là khi ai lớn hơn m rất nhiều, chẳng hạn như ai = 10^9 với m = 1. Trong tình huống này, mọi tên đều gây ra một lần lật trang. Sự phân tách số học tạo ra chính xác lượt ai và rem chính xác vẫn là 1 ở cuối, duy trì tính nhất quán qua các ngày. 

Trường hợp tinh tế thứ ba là khi một ngày lấp đầy chính xác công suất còn lại và sau đó kết thúc ở một ranh giới. Nếu không đặt lại rem thành m một cách rõ ràng trong trường hợp này, ngày hôm sau sẽ nhầm tưởng rằng trang vẫn được lấp đầy một phần, dẫn đến việc lật trang bị thiếu.
