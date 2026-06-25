---
title: "CF 105257A - chmod"
description: "Mỗi dòng đầu vào mô tả cấu hình quyền giống Unix được mã hóa ở dạng số nhỏ gọn. Thay vì trực tiếp cấp quyền cho một tệp, hệ thống sử dụng một số có ba chữ số, trong đó mỗi chữ số mô tả độc lập quyền truy cập cho một trong ba lớp người dùng…"
date: "2026-06-24T04:25:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105257
codeforces_index: "A"
codeforces_contest_name: "2024 ICPC ShaanXi Provincial Contest"
rating: 0
weight: 105257
solve_time_s: 53
verified: true
draft: false
---

[CF 105257A - chmod](https://codeforces.com/problemset/problem/105257/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả cấu hình quyền giống Unix được mã hóa ở dạng số nhỏ gọn. Thay vì trực tiếp cấp quyền cho một tệp, hệ thống sử dụng một số có ba chữ số, trong đó mỗi chữ số mô tả độc lập quyền truy cập cho một trong ba lớp người dùng: chủ sở hữu, nhóm và những người khác. 

Mỗi chữ số nằm trong khoảng từ 0 đến 7 và phải được hiểu là mặt nạ 3 bit. Ba bit tương ứng, theo thứ tự cố định, để đọc, ghi và thực thi các quyền. Nếu một bit được đặt, ký tự tương ứng sẽ xuất hiện trong chuỗi quyền cuối cùng, nếu không thì dấu gạch ngang sẽ được sử dụng. 

Đầu ra của mỗi trường hợp thử nghiệm là một chuỗi 9 ký tự. Nó được hình thành bằng cách mở rộng từng chữ số trong số ba chữ số thành một khối gồm ba ký tự và ghép các khối theo chủ sở hữu đơn hàng, nhóm, những người khác. 

Các ràng buộc là nhỏ, tối đa là 100 trường hợp thử nghiệm và công việc không đổi cho mỗi trường hợp. Điều này ngay lập tức loại trừ bất kỳ điều gì ngoài việc xử lý tuyến tính trên mỗi dòng đầu vào, mặc dù ngay cả hành vi bậc hai vẫn sẽ vượt qua. Cấu trúc của bài toán là giải mã có chiều rộng cố định nên hiệu suất không phải là khó khăn chính. 

Một lỗi phổ biến xuất phát từ việc hiểu sai thứ tự bit. Bit quan trọng nhất tương ứng với việc đọc, bit ở giữa để ghi và bit ít quan trọng nhất để thực thi. Ví dụ: chữ số 6 tương ứng với số nhị phân 110, nghĩa là cho phép đọc và ghi nhưng không thực thi được, tạo ra chuỗi “rw-”. Một lỗi thường gặp khác là đảo ngược thứ tự các ký tự trong mỗi bộ ba, dẫn đến hoán vị không chính xác như “wr-” hoặc “-rw”. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi chữ số là một số từ 0 đến 7 và kiểm tra rõ ràng từng quyền bằng cách sử dụng logic chuyển đổi nhị phân hoặc modulo lặp lại. Đối với mỗi chữ số, người ta có thể chia liên tục cho 2 hoặc trích xuất từng bit một, xây dựng chuỗi thành một mảng tạm thời. Điều này đúng vì mỗi chữ số mã hóa chính xác ba cờ boolean độc lập. 

Cách tiếp cận này đã chạy với thời gian không đổi trên mỗi chữ số vì chỉ có ba bit, nhưng nó có xu hướng được viết dài dòng hơn và dễ mắc lỗi hơn. Nếu được thực hiện một cách bất cẩn bằng cách sử dụng các thao tác chuỗi hoặc nối lặp lại bên trong các vòng lặp, nó có thể trở nên chậm hoặc lộn xộn một cách không cần thiết, mặc dù độ phức tạp về mặt lý thuyết vẫn không đáng kể. 

Quan sát quan trọng là mỗi chữ số ánh xạ trực tiếp tới ba vị trí bit cố định. Thay vì mô phỏng trích xuất nhị phân, chúng ta có thể sử dụng các phép toán theo bit. Điều này làm giảm giải pháp tra cứu trực tiếp xem mỗi bit trong số ba bit có được đặt hay không. Vì chỉ có tám giá trị có thể có cho mỗi chữ số nên việc ánh xạ hoàn toàn mang tính xác định và thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khai thác bit Brute Force | O(T) | O(1) | Đã chấp nhận | 
| Ánh xạ trực tiếp theo bit | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và lặp lại từng chuỗi đầu vào. 
2. Đối với mỗi ký tự trong chuỗi, hãy chuyển đổi nó thành một chữ số nguyên. 
3. Với mỗi chữ số, xác định các ký tự cho phép theo thứ tự cố định: đọc, viết, thực thi. 
4. Đối với mỗi quyền, hãy kiểm tra bit tương ứng trong chữ số bằng cách sử dụng bit AND. 
5. Nếu bit được đặt, hãy thêm ký tự tương ứng, nếu không hãy thêm dấu gạch ngang. 
6. Ghép các khối ba ký tự cho tất cả các chữ số và xuất ra chuỗi 9 ký tự kết quả. 

Lý do thứ tự này hoạt động là vì vấn đề xác định ánh xạ vị trí nghiêm ngặt giữa các bit và loại quyền. Mỗi chữ số là độc lập nên việc giải mã có thể được thực hiện cục bộ mà không có bất kỳ sự phụ thuộc chéo nào giữa chủ sở hữu, nhóm và những người khác. 

### Tại sao nó hoạt động

Mỗi chữ số mã hóa một tập hợp con của bộ ba phần tử, trong đó mỗi phần tử tương ứng với một loại quyền. Biểu diễn nhị phân của chữ số chính xác là vectơ đặc tính của tập hợp con đó. Vì ánh xạ từ bit tới ký tự là cố định và nội động, nên việc xây dựng lại chuỗi bằng cách kiểm tra từng bit một cách độc lập sẽ bảo toàn tất cả thông tin mà không bị mơ hồ. Việc ghép các khối được giải mã sẽ tái tạo lại chuỗi quyền đầy đủ một cách duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def decode_digit(d):
    r = 'r' if d & 4 else '-'
    w = 'w' if d & 2 else '-'
    x = 'x' if d & 1 else '-'
    return r + w + x

t = int(input())
out = []

for _ in range(t):
    s = input().strip()
    res = []
    for ch in s:
        d = ord(ch) - ord('0')
        res.append(decode_digit(d))
    out.append(''.join(res))

print('\n'.join(out))
```Cốt lõi của việc thực hiện là`decode_digit`hàm tách biệt ba bit quyền bằng cách sử dụng bit AND. Các hằng số 4, 2 và 1 tương ứng với trọng số nhị phân của các vị trí đọc, ghi và thực thi tương ứng. 

Vòng lặp chính chỉ áp dụng phép biến đổi này một cách độc lập cho từng ký tự và nối các kết quả. Việc sử dụng danh sách để tích lũy sẽ tránh được chi phí nối chuỗi lặp lại, điều này không cần thiết nhưng vẫn là một thói quen tốt trong lập trình cạnh tranh Python. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào bao gồm hai trường hợp thử nghiệm:```
2
760
123
```Đối với trường hợp đầu tiên, chúng tôi giải mã từng chữ số riêng biệt. 

| Chữ số | Nhị phân | Đọc | Viết | Thực hiện | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 7 | 111 | r | w | x | rwx | 
| 6 | 110 | r | w | - | rw- | 
| 0 | 000 | - | - | - | --- | 

Nối mang lại cho`rwxrw----`. 

Đối với trường hợp thứ hai: 

| Chữ số | Nhị phân | Đọc | Viết | Thực hiện | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 001 | - | - | x | --x | 
| 2 | 010 | - | w | - | -w- | 
| 3 | 011 | - | w | x | -wx | 

Nối mang lại cho`--x-w--wx`. 

Những dấu vết này cho thấy mỗi chữ số được giải mã độc lập và luôn tạo ra chính xác 3 ký tự, giữ nguyên cấu trúc cố định trên tất cả các đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp kiểm thử xử lý chính xác ba chữ số với các phép toán bit có thời gian không đổi | 
| Không gian | O(T) | Bộ nhớ đầu ra chiếm ưu thế vì chúng tôi xây dựng một chuỗi cho mỗi trường hợp thử nghiệm | 

Thời gian chạy là tuyến tính theo số lượng trường hợp thử nghiệm và hệ số không đổi là cực kỳ nhỏ. Với tối đa 100 đầu vào, việc thực thi sẽ diễn ra ngay lập tức trong điều kiện hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def decode_digit(d):
        r = 'r' if d & 4 else '-'
        w = 'w' if d & 2 else '-'
        x = 'x' if d & 1 else '-'
        return r + w + x

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        res = []
        for ch in s:
            d = ord(ch) - ord('0')
            res.append(decode_digit(d))
        out.append(''.join(res))

    return '\n'.join(out)

# provided sample (interpreted)
assert run("2\n760\n123\n") == "rwxrw----\n--x-w--wx"

# minimal case
assert run("1\n0\n") == "---------"

# all ones
assert run("1\n111\n") == "--x--x--x"

# full permissions
assert run("1\n777\n") == "rwxrwxrwx"

# mixed case
assert run("1\n504\n") == "r-x---r--"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0 | --------- | tắt tất cả các quyền | 
| 1\n777 | rwxrwxrwx | tất cả các quyền trên | 
| 1\n111 | --x--x--x | truyền bá thực thi bit đơn | 
| 1\n504 | r-x---r-- | chữ số độc lập hỗn hợp | 

## Vỏ cạnh 

Đối với một đầu vào như`0`, chữ số không có bit nào được đặt, vì vậy cả ba quyền đều ánh xạ tới dấu gạch ngang. Thuật toán đánh giá`0 & 4`,`0 & 2`, Và`0 & 1`, tất cả đều sai, tạo ra “---” cho mỗi nhóm. Việc lặp lại điều này ba lần sẽ mang lại một chuỗi đầy đủ chín dấu gạch ngang, khớp với trạng thái quyền trống dự định. 

Đối với một đầu vào như`7`, tất cả các bit được thiết lập. Séc`7 & 4`,`7 & 2`, Và`7 & 1`tất cả đều đánh giá là đúng, tạo ra “rwx”. Hành vi này nhất quán trên tất cả các chữ số và vì mỗi chữ số được xử lý độc lập nên không có sự can thiệp giữa các nhóm, đảm bảo tính chính xác ngay cả trong các trường hợp thống nhất hoặc cực đoan.
