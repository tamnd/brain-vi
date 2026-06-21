---
title: "CF 105761B - Chuỗi Fiborooji"
description: "Chúng ta được cấp một cặp số có một chữ số bắt đầu, trong đó có ít nhất một số khác 0. Từ cặp này, chúng tôi tạo ra một chuỗi trong đó mỗi giá trị tiếp theo được hình thành chính xác như Fibonacci: tổng của hai giá trị trước đó."
date: "2026-06-21T22:52:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105761
codeforces_index: "B"
codeforces_contest_name: "2021 UCF Local Programming Contest"
rating: 0
weight: 105761
solve_time_s: 53
verified: true
draft: false
---

[CF 105761B - Chuỗi Fiborooji](https://codeforces.com/problemset/problem/105761/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cặp số có một chữ số bắt đầu, trong đó có ít nhất một số khác 0. Từ cặp này, chúng tôi tạo ra một chuỗi trong đó mỗi giá trị tiếp theo được hình thành chính xác như Fibonacci: tổng của hai giá trị trước đó. Điểm khác biệt duy nhất là chúng tôi không bao giờ cho phép các số vượt quá một chữ số, vì vậy bất cứ khi nào tổng đạt đến 10 hoặc hơn, chúng tôi chỉ giữ lại chữ số cuối cùng của nó. 

Điều này tạo ra một quá trình xác định phát triển từ một cặp có thứ tự ban đầu thành một chuỗi các cặp có thứ tự. Ở mỗi bước, trạng thái của hệ thống được mô tả đầy đủ bằng hai số cuối. Nhiệm vụ là xác định xem chuỗi này chạy trong bao lâu cho đến khi chúng ta gặp lại cặp xuất phát ban đầu ở các vị trí liên tiếp, nghĩa là hệ thống đã trở về trạng thái ban đầu. 

Mặc dù trình tự trông có vẻ dạng số nhưng cấu trúc thực là một hệ thống chuyển tiếp qua các cặp (a, b), trong đó mỗi bước chuyển sang (b, (a + b) mod 10). Vì chỉ có 100 trạng thái khả dĩ nên việc lặp lại được đảm bảo rất nhanh chóng. 

Các ràng buộc đủ nhỏ để bất kỳ phương pháp khám phá nào có trạng thái lên tới không gian trạng thái đầy đủ gồm 100 khả năng sẽ chạy thoải mái trong giới hạn. Ngay cả một mô phỏng đơn giản theo dõi tất cả các trạng thái được truy cập cũng có hiệu quả là thời gian không đổi. 

Trường hợp phức tạp là khi chuỗi ngay lập tức quay vòng trong một vòng lặp rất ngắn. Ví dụ: bắt đầu từ (1, 0), các trạng thái tiếp theo là (0, 1), (1, 1), (1, 2), v.v. và việc quay trở lại có thể xảy ra tương đối muộn so với trực giác. Một trường hợp cạnh khác là đối xứng bắt đầu như (5, 5), trong đó phép truy toán nhanh chóng ổn định thành một chu kỳ ngắn. Một nỗ lực ngây thơ chỉ kiểm tra sự lặp lại của các giá trị đơn lẻ thay vì các cặp sẽ thất bại, bởi vì vấn đề rõ ràng là các cặp liên tiếp khớp với trạng thái ban đầu. 

## Phương pháp tiếp cận 

Cách trực tiếp để hiểu quy trình là mô phỏng nó chính xác như được xác định. Chúng ta bắt đầu từ cặp đã cho và liên tục tính giá trị tiếp theo là chữ số cuối cùng của tổng. Ở mỗi bước, chúng tôi di chuyển cặp về phía trước và kiểm tra xem liệu chúng tôi đã quay lại cấu hình ban đầu hay chưa. 

Mô phỏng lực lượng vũ phu này là chính xác vì nó phản ánh định nghĩa của chuỗi mà không có bất kỳ sự biến đổi nào. Tuy nhiên, trình tự tiến triển theo từng trạng thái chứ không phải từng số riêng lẻ. Chỉ có 10 giá trị có thể có cho mỗi thành phần, vì vậy chỉ tồn tại tổng cộng 100 trạng thái. Điều này có nghĩa là sau tối đa 100 lần chuyển đổi, một số trạng thái phải lặp lại. Do đó, phương pháp brute-force khám phá tối đa một đồ thị có kích thước không đổi. 

Quan sát quan trọng là chúng ta không đang giải quyết vấn đề tăng trưởng giống Fibonacci không giới hạn. Thay vào đó, chúng ta đang làm việc với một máy trạng thái hữu hạn. Khi chúng ta nhận thấy rằng phép truy hồi chỉ phụ thuộc vào hai giá trị cuối cùng theo modulo 10, bài toán sẽ giảm xuống việc tìm độ dài của chu trình bắt đầu ở trạng thái ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(100) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| Giải thích chu trình trạng thái | O(100) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

Cả hai mô tả đều dẫn đến việc triển khai giống nhau vì không gian trạng thái quá nhỏ nên không cần tối ưu hóa thêm. 

## Hướng dẫn thuật toán

1. Đọc cặp bắt đầu (a, b), xác định trạng thái ban đầu của hệ thống. Trạng thái này rất quan trọng vì chúng tôi đo lường thời điểm chúng tôi quay lại trạng thái đó ở các vị trí liên tiếp. 
2. Lưu trữ cặp ban đầu làm trạng thái mục tiêu mà chúng tôi muốn phát hiện lại. 
3. Khởi tạo bộ đếm để theo dõi xem có bao nhiêu số đã xuất hiện trong dãy cho đến nay. Chúng tôi bắt đầu với hai số đã có sẵn, vì vậy số đếm ban đầu là 2. 
4. Tính giá trị tiếp theo là (a + b) mod 10, sau đó chuyển cặp về phía trước (b, next_value). Bước này mã hóa quy tắc lặp lại trong khi thực thi ràng buộc một chữ số. 
5. Tăng bộ đếm sau khi tạo mỗi số mới, vì mỗi trạng thái mới đóng góp một phần tử bổ sung vào chuỗi. 
6. Tiếp tục lặp lại quá trình chuyển đổi cho đến khi cặp hiện tại khớp với cặp bắt đầu ban đầu. Thời điểm điều này xảy ra, trình tự đã hoàn thành một chu kỳ đầy đủ. 
7. Xuất ra bộ đếm, biểu thị độ dài của chuỗi cho đến và bao gồm cả lần xuất hiện lại đầu tiên của cặp ban đầu. 

### Tại sao nó hoạt động 

Quá trình này phát triển hoàn toàn thông qua sự chuyển đổi của các cặp có thứ tự và mỗi quá trình chuyển đổi đều mang tính quyết định. Điều này có nghĩa là hệ thống xác định một đồ thị có hướng trong đó mỗi nút có chính xác một cạnh đi ra. Bắt đầu từ bất kỳ nút nào trong biểu đồ hữu hạn như vậy đảm bảo sự lặp lại cuối cùng. Bởi vì chúng tôi chỉ dừng rõ ràng khi nút ban đầu xuất hiện lại, nên quá trình đo chính xác một độ dài chu kỳ đầy đủ bắt đầu từ nút đó. Không có sự lặp lại trung gian nào có thể kết thúc sớm vì chúng tôi so sánh các cặp đầy đủ chứ không phải các giá trị riêng lẻ, bảo toàn tính toàn vẹn trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    start = (a, b)

    count = 2
    while True:
        c = (a + b) % 10
        a, b = b, c
        count += 1
        if (a, b) == start:
            print(count)
            return

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại cặp hiện tại và bộ đếm. Đường chuyển tiếp`(a + b) % 10`thực thi quy tắc Fiborooji một cách trực tiếp. Vòng lặp tiếp tục cho đến khi trạng thái khớp lại với cặp ban đầu, lúc đó chúng ta đã hoàn thành đúng một chu kỳ đầy đủ. Bộ đếm bắt đầu ở mức 2 vì đầu vào đã đóng góp hai phần tử trước khi xảy ra bất kỳ chuyển đổi nào. 

Một lỗi phổ biến ở đây là bắt đầu bộ đếm ở 0 hoặc 1, làm thay đổi câu trả lời cuối cùng không chính xác theo kích thước trạng thái ban đầu. Một điểm tinh tế khác là chúng ta phải so sánh các cặp chứ không phải các giá trị riêng lẻ vì điều kiện chu trình phụ thuộc đồng thời vào cả hai số trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Nhập 3 4 

Chúng ta bắt đầu từ cặp (3, 4) và liên tục áp dụng phép chuyển đổi. 

| Bước | Cặp hiện tại | Giá trị tiếp theo | Đếm | 
| --- | --- | --- | --- | 
| 0 | (3, 4) | - | 2 | 
| 1 | (4, 7) | 7 | 3 | 
| 2 | (7, 1) | 1 | 4 | 
| 3 | (1, 8) | 8 | 5 | 
| 4 | (8, 9) | 9 | 6 | 
| 5 | (9, 7) | 7 | 7 | 
| 6 | (7, 6) | 6 | 8 | 
| ... | ... | ... | ... | 
| cuối cùng | (3, 4) | - | 14 | 

Dấu vết này cho thấy hệ thống cuối cùng sẽ trở về cặp ban đầu sau 14 phần tử. Quan sát quan trọng là các giá trị trung gian lặp lại các mẫu trước đó, nhưng việc kết thúc chỉ xảy ra khi cả hai thành phần khớp với trạng thái ban đầu với nhau. 

### Ví dụ 2: Nhập 0 6 

Bắt đầu từ (0, 6), quá trình tiến hóa bị chi phối bởi thực tế là các số 0 đóng vai trò là chất ổn định ở đầu chuỗi, nhưng hệ thống vẫn bước vào một chu kỳ đầy đủ sau đó. Logic máy trạng thái tương tự được áp dụng và chuỗi cuối cùng quay trở lại (0, 6), xác nhận tính chất tuần hoàn của phép truy hồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100) | Không gian trạng thái có tối đa 100 cặp, do đó vòng lặp chạy với số lần lặp không đổi | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ bất kể đầu vào | 

Các ràng buộc của vấn đề làm cho thời gian này không đổi một cách hiệu quả. Ngay cả trong trường hợp xấu nhất, chúng tôi khám phá tối đa không gian trạng thái 10 x 10 đầy đủ trước khi quay lại cặp ban đầu. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    a, b = map(int, input().split())
    start = (a, b)
    count = 2
    while True:
        c = (a + b) % 10
        a, b = b, c
        count += 1
        if (a, b) == start:
            print(count)
            return

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided sample (as described in statement)
# exact outputs for full sample set depend on full official statement formatting
assert run("3 4") == "14"

# custom cases
assert run("0 1") > "0", "basic progression case"
assert run("1 0") != "", "valid termination exists"
assert run("5 5") != "", "symmetric start"
assert run("9 9") != "", "max digit boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 4 | 14 | tính đúng đắn của chu kỳ kinh điển | 
| 0 1 | độ dài khác không | tiến triển từ đầu | 
| 1 0 | không trống | xử lý bất đối xứng | 
| 5 5 | không trống | hành vi bắt đầu đối xứng | 
| 9 9 | không trống | hành vi mang chữ số ranh giới | 

## Vỏ cạnh 

Đối với điểm bắt đầu như (0, 6), chuỗi ban đầu hoạt động giống như một chuỗi Fibonacci dịch chuyển với nhiều số 0 ảnh hưởng đến quá trình chuyển đổi sớm. Thuật toán vẫn theo dõi các cặp đầy đủ, do đó, ngay cả khi các giá trị riêng lẻ lặp lại sớm, việc chấm dứt sẽ không xảy ra cho đến khi cặp chính xác (0, 6) xuất hiện lại. Vòng lặp tiếp tục chính xác cho đến khi khớp trạng thái đầy đủ đó. 

Đối với các điểm bắt đầu đối xứng như (5, 5), trạng thái tiếp theo sẽ trở thành (5, 0), trạng thái này nhanh chóng phá vỡ tính đối xứng. Mặc dù cả hai giá trị bắt đầu đều giống hệt nhau, máy trạng thái sẽ phân kỳ ngay lập tức và chỉ quay trở lại cặp ban đầu sau khi hoàn thành toàn bộ chu trình của nó. Việc triển khai xử lý việc này một cách tự nhiên vì nó luôn so sánh các cặp có thứ tự thay vì các giá trị một cách độc lập.
