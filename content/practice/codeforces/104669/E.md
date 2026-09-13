---
title: "CF 104669E - Quay vòng"
description: "Chúng ta được cấp một số nguyên không âm và được yêu cầu diễn giải lại nó thông qua một phép biến đổi trên biểu diễn nhị phân của nó. Quá trình này được mô tả đơn giản nhưng hơi gián tiếp trong quá trình thực hiện."
date: "2026-06-29T09:41:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104669
codeforces_index: "E"
codeforces_contest_name: "Turtle Codes"
rating: 0
weight: 104669
solve_time_s: 55
verified: true
draft: false
---

[CF 104669E - Chuyển đổi](https://codeforces.com/problemset/problem/104669/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên không âm và được yêu cầu diễn giải lại nó thông qua một phép biến đổi trên biểu diễn nhị phân của nó. Quá trình này được mô tả đơn giản nhưng hơi gián tiếp trong quá trình thực hiện. Đầu tiên, chúng ta viết số ở dạng nhị phân mà không có bất kỳ số 0 nào đứng đầu, có nghĩa là chúng ta chỉ giữ các bit có ý nghĩa từ bit 1 có ý nghĩa nhất đến bit có ý nghĩa nhỏ nhất. Sau đó chúng ta đảo ngược thứ tự của các bit này thành một chuỗi. Cuối cùng, chúng tôi hiểu chuỗi bit đảo ngược này là một số nhị phân mới và chuyển đổi nó thành số nguyên cơ số 10. 

Điểm tinh tế quan trọng nhất là việc chuyển đổi hoàn toàn là vị trí trên các bit. Không có số học nào được thực hiện trên chính giá trị đó ngoài việc phân tách và tái cấu trúc nhị phân. Điều này có nghĩa là vấn đề cơ bản là về thao tác bit hơn là lý thuyết số hoặc tổ hợp. 

Ràng buộc về kích thước đầu vào, tối đa 10^18, ngụ ý rằng biểu diễn nhị phân có tối đa 60 bit. Bất kỳ giải pháp nào xử lý số trong thời gian O(log N) là đủ. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các hoạt động bit trong phạm vi tuyến tính lên đến N hoặc thực hiện các hoạt động chuỗi nặng lặp đi lặp lại trên các cấu trúc lớn vượt quá độ dài bit của số. 

Một sự hiểu lầm ngây thơ xuất hiện khi mọi người nghĩ rằng sự đảo ngược áp dụng cho biểu diễn nhị phân có chiều rộng cố định. Ví dụ: đệm đến 64 bit và đảo ngược sẽ tạo ra kết quả khác. Vấn đề cấm rõ ràng các số 0 đứng đầu trong biểu diễn ban đầu, do đó không được đưa phần đệm vào. 

Một trường hợp cạnh phổ biến khác là chính nó. Nếu đầu vào là 0 thì biểu diễn nhị phân của nó là "0". Đảo ngược vẫn mang lại "0" và đầu ra vẫn là 0. Bất kỳ triển khai nào giả định ít nhất một bit 1 dẫn đầu sẽ thất bại ở đây nếu nó không xử lý vấn đề này một cách rõ ràng. 

## Phương pháp tiếp cận 

Cách mạnh mẽ để suy nghĩ về vấn đề này là xây dựng một cách rõ ràng chuỗi nhị phân của số, đảo ngược nó và sau đó diễn giải lại nó thành số nhị phân. Việc chuyển đổi một số thành nhị phân mất thời gian O(log N) vì mỗi bước chia cho 2. Đảo ngược một chuỗi có độ dài log N cũng là O(log N) và việc xây dựng lại số từ chuỗi đảo ngược lại là O(log N). Điều này đã phù hợp thoải mái trong những hạn chế. 

Tuy nhiên, chúng ta có thể tránh hoàn toàn việc xây dựng các chuỗi đầy đủ bằng cách quan sát cấu trúc của sự đảo ngược. Khi chúng tôi trích xuất các bit từ phía có trọng số nhỏ nhất của N, các bit đó sẽ trở thành các bit có trọng số cao nhất ở đầu ra. Điều này có nghĩa là thay vì lưu trữ một chuỗi và đảo ngược nó, chúng ta có thể xây dựng kết quả tăng dần bằng cách dịch chuyển và thêm các bit theo thứ tự chúng ta trích xuất chúng. 

Cái nhìn sâu sắc cốt lõi là việc đảo ngược biểu diễn nhị phân tương đương với việc đọc các bit của N từ ít quan trọng nhất đến quan trọng nhất và xây dựng một số mới bằng cách dịch chuyển sang trái và chèn từng bit được trích xuất. Điều này loại bỏ mọi nhu cầu thao tác chuỗi và giữ cho toàn bộ quá trình hoàn toàn là số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(log N) | O(log N) | Đã chấp nhận | 
| Tối ưu | O(log N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu với số đã cho và kết quả trống được khởi tạo bằng 0. Kết quả sẽ tích lũy diễn giải nhị phân đảo ngược khi chúng tôi xử lý các bit. 
2. Trong khi số này lớn hơn 0, hãy liên tục trích xuất bit có trọng số nhỏ nhất của nó bằng cách sử dụng modulo 2 hoặc theo bit AND với 1. Bit này chính xác là ký hiệu tiếp theo trong chuỗi nhị phân đảo ngược. 
3. Dịch chuyển kết quả sang trái một vị trí để tạo khoảng trống cho bit mới. Điều này đảm bảo các bit được trích xuất trước đó chiếm vị trí có ý nghĩa cao hơn trong số cuối cùng. 
4. Thêm bit được trích xuất vào kết quả. Điều này nối thêm chữ số nhị phân đảo ngược vào đúng vị trí của nó. 
5. Dịch chuyển số ban đầu sang phải một bit để loại bỏ bit đã xử lý và chuyển sang bit tiếp theo. 
6. Tiếp tục cho đến khi hết bit. Kết quả xây dựng được là câu trả lời cuối cùng. 

Đối với trường hợp đặc biệt khi đầu vào bằng 0, vòng lặp không thực thi và đầu ra đúng trực tiếp bằng 0. 

### Tại sao nó hoạt động 

Ở mỗi lần lặp, chúng tôi mô phỏng một cách hiệu quả việc xây dựng chuỗi bit đảo ngược. Điều bất biến là sau khi xử lý k bit, kết quả chứa chính xác tiền tố đảo ngược của k bit cuối cùng của biểu diễn nhị phân ban đầu, theo đúng thứ tự. Vì mỗi bit mới từ số ban đầu được thêm vào phía ít quan trọng nhất của kết quả sau khi dịch trái, nên tính chính xác về vị trí được duy trì trong suốt quá trình. Không có bit nào bị ghi đè hoặc đặt sai vị trí và quá trình kết thúc chính xác khi tất cả các bit quan trọng đã được tiêu thụ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    res = 0

    while n > 0:
        res = (res << 1) | (n & 1)
        n >>= 1

    print(res)

if __name__ == "__main__":
    solve()
```Việc triển khai đọc số, sau đó liên tục sử dụng biểu diễn nhị phân của nó từ bit có trọng số thấp nhất đến bit có trọng số cao nhất. Mỗi bước dịch chuyển kết quả sang trái và chèn bit được trích xuất. Điều này trực tiếp xây dựng cách giải thích nhị phân đảo ngược mà không bao giờ xây dựng một chuỗi một cách rõ ràng. 

Một điểm tinh tế là thứ tự các thao tác trong bước cập nhật. biểu hiện`(res << 1) | (n & 1)`đảm bảo rằng kết quả hiện tại được dịch chuyển trước khi chèn bit mới, bảo toàn cấu trúc vị trí. Việc sử dụng phép cộng thay vì theo bit OR cũng sẽ có tác dụng ở đây vì bit được chèn luôn là 0 hoặc 1, nhưng OR có mục đích rõ ràng hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```Nhị phân của 5 là`101`. 

Chúng tôi xử lý các bit từ phía ít quan trọng nhất. 

| Bước | n (nhị phân) | trích xuất bit | res (nhị phân) | 
| --- | --- | --- | --- | 
| 1 | 101 | 1 | 1 | 
| 2 | 10 | 0 | 10 | 
| 3 | 1 | 1 | 101 | 

Sau khi xử lý tất cả các bit, kết quả là`101`, là 5 ở dạng thập phân. 

Điều này cho thấy các mẫu nhị phân đối xứng không thay đổi khi đảo ngược và thuật toán bảo toàn cấu trúc đó một cách tự nhiên. 

### Ví dụ 2 

đầu vào:```
731053868524
```Chúng ta không cần phải mở rộng hoàn toàn cách biểu diễn nhị phân; thay vào đó chúng tôi tuân theo logic trích xuất tương tự về mặt khái niệm. 

| Tóm tắt bước | Hành vi | 
| --- | --- | 
| Bắt đầu | độ phân giải = 0 | 
| Quá trình bit 0 | res giữ nguyên 0 hoặc dịch chuyển không thay đổi tùy theo bit | 
| Xử lý chuỗi 60 bit đầy đủ | res tích lũy thứ tự bit đảo ngược | 

Kết quả tính toán cuối cùng là:```
238960798805
```Điều này chứng tỏ rằng ngay cả đối với đầu vào lớn, thuật toán vẫn hoàn toàn tuyến tính theo số bit chứ không phải độ lớn của số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log N) | Mỗi lần lặp sẽ loại bỏ một bit nhị phân khỏi số | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến số nguyên không đổi | 

Thời gian chạy được giới hạn bởi số bit trong đầu vào, tối đa là khoảng 60 đối với các ràng buộc đã cho. Điều này đảm bảo giải pháp thực hiện thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    n = int(sys.stdin.readline().strip())
    res = 0
    while n > 0:
        res = (res << 1) | (n & 1)
        n >>= 1
    print(res)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided samples
assert run("5\n") == "5", "sample 1"
assert run("731053868524\n") == "238960798805", "sample 2"

# custom cases
assert run("0\n") == "0", "zero case"
assert run("1\n") == "1", "single bit"
assert run("2\n") == "1", "10 -> 01"
assert run("6\n") == "3", "110 -> 011"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | xử lý số 0 mà không cần lặp | 
| 1 | 1 | ổn định bit đơn | 
| 2 | 1 | số 0 đứng đầu ở dạng đảo ngược | 
| 6 | 3 | độ chính xác đảo ngược nhiều bit | 

## Vỏ cạnh 

Đầu vào 0 là trường hợp suy biến cấu trúc duy nhất vì nó không tạo ra sự lặp lại trong vòng lặp chính. Đối với đầu vào`0`, biểu diễn nhị phân là`0`và thuật toán ngay lập tức đưa ra`0`vì bộ tích lũy không bao giờ được sửa đổi. 

Đối với đầu vào`1`, biểu diễn nhị phân là một bit đơn. Vòng lặp chạy một lần, trích xuất`1`, và đặt nó vào kết quả, mang lại`1`. Điều này xác nhận rằng thuật toán xử lý chính xác cấu trúc khác 0 tối thiểu mà không yêu cầu cách viết đặc biệt. 

Đối với đầu vào là lũy thừa của hai, chẳng hạn như`2`hoặc`8`, biểu diễn nhị phân có một bit cao duy nhất theo sau là số 0. Đảo ngược di chuyển bit cao đó đến vị trí ít quan trọng nhất. Vòng lặp đạt được điều này một cách tự nhiên vì các số 0 góp phần dịch chuyển mà không cần thiết lập các bit và bit 1 cuối cùng kết thúc ở vị trí đảo ngược chính xác.
