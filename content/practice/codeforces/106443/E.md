---
title: "CF 106443E - Đánh giá"
description: "Nhiệm vụ là một vấn đề chuyển đổi định dạng trực tiếp trên một lưới có kích thước cố định đại diện cho một bàn cờ. Đầu vào là một ma trận 8 x 8 ký tự, trong đó mỗi ô là một quân cờ hoặc một ô vuông trống."
date: "2026-06-21T16:24:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "E"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 62
verified: true
draft: false
---

[CF 106443E - Đánh giá](https://codeforces.com/problemset/problem/106443/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là một vấn đề chuyển đổi định dạng trực tiếp trên một lưới có kích thước cố định đại diện cho một bàn cờ. Đầu vào là một ma trận 8 x 8 ký tự, trong đó mỗi ô là một quân cờ hoặc một ô vuông trống. Các quân cờ được mã hóa bằng các ký hiệu cờ vua tiêu chuẩn, chữ hoa cho quân trắng và chữ thường cho quân đen, trong khi các ô trống được biểu thị bằng dấu chấm. 

Đầu ra được yêu cầu là một chuỗi đơn ở dạng Ký hiệu Forsyth-Edwards (FEN). FEN nén từng hàng của bảng từ trên xuống dưới, thay thế các ô trống liên tiếp bằng số lượng của chúng và giữ nguyên các ký tự quân cờ. Các hàng được phân tách bằng dấu gạch chéo. 

Cấu trúc của đầu vào đã khớp với bố cục không gian của bảng FEN, do đó, vấn đề giảm xuống mức nén theo hàng với quy tắc mã hóa độ dài chạy đơn giản chỉ áp dụng cho dấu chấm. 

Các ràng buộc là tối thiểu vì kích thước bảng được cố định ở mức 8 x 8. Điều này có nghĩa là bất kỳ giải pháp nào, ngay cả giải pháp có vòng lặp lồng nhau trên toàn bộ lưới, đều chạy trong thời gian không đổi. Không cần phải tối ưu hóa tiệm cận. Yêu cầu thực sự duy nhất là tính chính xác trong việc xử lý các ô trống và dấu phân cách hàng liên tiếp. 

Những lỗi phổ biến nhất đến từ các vấn đề định dạng tinh tế. Một lỗi thường gặp là quên xóa số chấm đang chờ xử lý ở cuối hàng. Ví dụ, hãy xem xét hàng```
........
```Đầu ra đúng là`8`, nhưng việc triển khai đơn giản chỉ phát ra số đếm khi gặp ký tự không có dấu chấm sẽ tạo ra một chuỗi trống không chính xác trừ khi nó xử lý rõ ràng việc xóa cuối hàng. 

Một trường hợp khác là trộn các chữ số và ký tự. Ví dụ,```
r.bk...r
```phải trở thành`r1bk3r`. Cách tiếp cận có lỗi có thể nối trực tiếp chữ số mà không xem xét rằng nhiều phân đoạn dấu chấm phải được tổng hợp độc lập. 

Cuối cùng, việc phân tách hàng phải chính xác. Thiếu dấu gạch chéo giữa các hàng hoặc thêm dấu gạch chéo ở cuối sẽ tạo ra FEN không hợp lệ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng xử lý từng ô và đưa ra kết quả đầu ra ngay lập tức cho mọi ký tự. Người ta có thể thêm các dấu chấm nguyên trạng và sau đó cố gắng xử lý hậu kỳ chuỗi để nén các dấu chấm. Điều này sẽ liên quan đến việc quét lại toàn bộ chuỗi trung gian để tính toán độ dài chạy, việc này không cần thiết và dễ xảy ra lỗi. 

Sự kém hiệu quả trong cách tiếp cận đó không phải là độ phức tạp về thời gian theo nghĩa tiệm cận, vì bảng có kích thước không đổi, mà là độ phức tạp về cấu trúc: nhiều lượt và bộ nhớ bổ sung làm tăng khả năng nhóm không chính xác qua các ranh giới hàng. 

Quan sát chính là mỗi hàng là độc lập và việc nén dấu chấm hoàn toàn cục bộ trong một hàng. Điều này có nghĩa là chúng ta có thể xử lý từng hàng trong một lần quét tuyến tính duy nhất, duy trì bộ đếm cho các dấu chấm liên tiếp. Khi gặp một ký tự không phải dấu chấm, chúng ta sẽ xóa bộ đếm nếu nó khác 0, sau đó phát ra ký tự đó. Ở cuối hàng, chúng tôi xóa mọi bộ đếm còn lại và nối thêm kết quả hàng. 

Điều này làm giảm vấn đề xuống còn một lượt trên mỗi hàng với trạng thái bổ sung không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force với xử lý hậu kỳ | O(64) | O(64) | Được chấp nhận nhưng không cần thiết | 
| Nén hàng một lần | O(64) | O(64) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chính xác tất cả 8 hàng của bảng là các chuỗi có độ dài 8. Mỗi hàng tương ứng với một hạng trong thứ tự đầu ra FEN nên không cần đảo ngược. Điều này duy trì thứ tự từ trên xuống dưới theo yêu cầu của ký hiệu. 
2. Khởi tạo một danh sách trống để tích lũy các hàng được mã hóa. Việc sử dụng danh sách sẽ tránh được chi phí nối chuỗi lặp lại, ngay cả khi kích thước đầu vào nhỏ. 
3. Đối với mỗi hàng, hãy khởi tạo bộ đếm cho các dấu chấm liên tiếp và trình tạo chuỗi trống cho phiên bản được mã hóa của hàng đó. 
4. Quét ký tự từ trái sang phải. Nếu ký tự hiện tại là dấu chấm, hãy tăng bộ đếm dấu chấm. Điều này trì hoãn đầu ra cho đến khi chúng ta biết toàn bộ chiều dài của đoạn trống. 
5. Nếu ký tự hiện tại không phải là dấu chấm, trước tiên hãy kiểm tra xem bộ đếm dấu chấm có khác 0 hay không. Nếu đúng như vậy, hãy thêm giá trị số của nó vào chuỗi hàng và đặt lại bộ đếm. Sau đó nối thêm ký tự mảnh đó. 
6. Sau khi kết thúc quá trình quét một hàng, hãy thực hiện lần xóa cuối cùng của bộ đếm dấu chấm nếu nó khác 0. Điều này đảm bảo các đoạn trống ở cuối không bị mất. 
7. Nối hàng được mã hóa vào danh sách các hàng. 
8. Nối tất cả các hàng được mã hóa bằng dấu gạch chéo để tạo thành chuỗi FEN cuối cùng. 

### Tại sao nó hoạt động 

Mỗi hàng được phân tách thành các đoạn liền kề tối đa có dạng dấu chấm hoặc phần. Thuật toán thay thế từng đoạn dấu chấm tối đa bằng độ dài của nó và giữ nguyên tất cả các ký tự khác. Vì việc phân đoạn được thực hiện hoàn toàn từ trái sang phải mà không bị trùng lặp nên mỗi ô vuông sẽ đóng góp chính xác một lần cho đúng một phân đoạn. Tính độc lập của các hàng đảm bảo không có sự tương tác giữa các phân đoạn trên ranh giới hàng, do đó, tính chính xác cục bộ trên mỗi hàng hàm ý tính chính xác toàn cục của chuỗi FEN đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def encode_row(row: str) -> str:
    res = []
    cnt = 0

    for ch in row:
        if ch == '.':
            cnt += 1
        else:
            if cnt:
                res.append(str(cnt))
                cnt = 0
            res.append(ch)

    if cnt:
        res.append(str(cnt))

    return ''.join(res)

def main():
    rows = [input().strip() for _ in range(8)]
    encoded = [encode_row(r) for r in rows]
    print('/'.join(encoded))

if __name__ == "__main__":
    main()
```Giải pháp này tách mã hóa hàng thành một hàm trợ giúp, giúp logic nén dấu chấm có thể tái sử dụng và dễ lý giải hơn. Chi tiết triển khai quan trọng là lần xóa cuối cùng của bộ đếm dấu chấm sau mỗi lần quét hàng; không có nó, các ô vuông trống sẽ không bao giờ được phát ra. 

Sử dụng danh sách để`res`tránh hành vi bậc hai từ việc nối chuỗi lặp lại, mặc dù tỷ lệ được cố định ở đây. Việc nối ở cuối sẽ tạo ra biểu diễn thu gọn cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
rnbqkbnr
pppppppp
........
........
........
........
PPPPPPPP
RNBQKBNR
```Xử lý từng hàng: 

| Hàng | Chấm chạy | Đã mã hóa | 
| --- | --- | --- | 
| rnbqkbnr | không | rnbqkbnr | 
| pppppppp | không | pppppppp | 
| ........ | 8 | 8 | 
| ........ | 8 | 8 | 
| ........ | 8 | 8 | 
| ........ | 8 | 8 | 
| PPPPPPPP | không | PPPPPPPP | 
| RNBQKBNR | không | RNBQKBNR | 

Đầu ra cuối cùng:```
rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR
```Dấu vết này cho thấy việc nén chính xác các hàng trống đầy đủ và xác nhận rằng ranh giới hàng được giữ nguyên. 

### Ví dụ 2 

đầu vào:```
r.bk...r
........
```| Hàng | Chấm chạy | Đã mã hóa | 
| --- | --- | --- | 
| r.bk...r | 1, 3 | r1bk3r | 
| ........ | 8 | 8 | 

Đầu ra cuối cùng:```
r1bk3r/8
```Ví dụ này xác minh rằng nhiều phân đoạn trống riêng biệt được xử lý độc lập và các phân đoạn cuối được xóa chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(64) | Mỗi hàng trong số 8 hàng có độ dài 8 được quét một lần | 
| Không gian | O(1) | Chỉ lưu trữ bổ sung liên tục ngoài bộ đệm đầu ra | 

Việc tính toán được giới hạn nghiêm ngặt bởi một lưới 8 x 8 cố định, do đó nó chạy ngay lập tức trong giới hạn nhất định. Việc sử dụng bộ nhớ không đổi ngoài chính chuỗi đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def encode_row(row: str) -> str:
        res = []
        cnt = 0
        for ch in row:
            if ch == '.':
                cnt += 1
            else:
                if cnt:
                    res.append(str(cnt))
                    cnt = 0
                res.append(ch)
        if cnt:
            res.append(str(cnt))
        return ''.join(res)

    rows = [input().strip() for _ in range(8)]
    print('/'.join(encode_row(r) for r in rows))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample 1
assert run("""rnbqkbnr
pppppppp
........
........
........
........
PPPPPPPP
RNBQKBNR
""") == "rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR"

# provided sample 2
assert run("""r.bk...r
........
........
........
........
........
........
........
""") == "r1bk3r/8/8/8/8/8/8/8"

# single piece
assert run("""........
........
........
...K....
........
........
........
........
""") == "8/8/8/3K4/8/8/8/8"

# alternating pattern
assert run("""r.r.r.r.
........
........
........
........
........
........
........
""") == "r1r1r1r1/8/8/8/8/8/8/8"

# full pieces
assert run("""RNBQKBNR
RNBQKBNR
RNBQKBNR
RNBQKBNR
RNBQKBNR
RNBQKBNR
RNBQKBNR
RNBQKBNR
""") == "RNBQKBNR/RNBQKBNR/RNBQKBNR/RNBQKBNR/RNBQKBNR/RNBQKBNR/RNBQKBNR/RNBQKBNR"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hàng bảng trống đầy đủ | 8/... | nén toàn bộ | 
| hỗn hợp chấm và mảnh | r1bk3r | nhiều phân khúc | 
| mảnh duy nhất ở trung tâm | 3K4 | tính chính xác của ranh giới | 
| mô hình xen kẽ | r1r1r1r1 | logic tuôn ra lặp đi lặp lại | 
| bảng đầy đủ | hàng không thay đổi | bảo tồn danh tính | 

## Vỏ cạnh 

Một hàng hoàn toàn trống như`........`kiểm tra xem việc triển khai có xóa chính xác bộ đếm dấu chấm ở cuối hàng hay không. Thuật toán tích lũy tám dấu chấm, sau đó nối thêm`8`khi hàng kết thúc, đảm bảo không mất trạng thái. 

Một hàng có nhiều vùng trống riêng biệt như`r.bk...r`xác minh rằng bộ đếm đặt lại chính xác giữa các phân đoạn. Quá trình quét tạo ra`r`, sau đó xả nước`1`, sau đó`bk`, sau đó xả nước`3`, và cuối cùng`r`, sản xuất`r1bk3r`. 

Một hàng gồm toàn các phần đảm bảo rằng bộ đếm dấu chấm vẫn bằng 0 xuyên suốt, do đó không có chữ số ngẫu nhiên nào được chèn vào.
