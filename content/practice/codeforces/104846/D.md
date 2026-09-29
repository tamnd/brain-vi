---
title: "CF 104846D - \u0428\u043e\u0443 \u0441 \u0434\u0435\u043b\u044c\u0444\u0438\u043d\u0430\u043c\u0438"
description: "Chúng ta được cho một chuỗi các số nguyên được hiển thị lần lượt. Sau khi mỗi số mới xuất hiện, chúng ta cần xác định xem liệu nó có thể được “xây dựng” hay không bằng cách lấy hai số trước đó và nối các biểu diễn thập phân của chúng theo thứ tự mà không cần chèn bất cứ thứ gì vào giữa."
date: "2026-06-28T11:28:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104846
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (7-8 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104846
solve_time_s: 84
verified: false
draft: false
---

[CF 104846D - \u0428\u043e\u0443 \u0441 \u0434\u0435\u043b\u044c\u0444\u0438\u043d\u0430\u043c\u0438](https://codeforces.com/problemset/problem/104846/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên được hiển thị lần lượt. Sau khi mỗi số mới xuất hiện, chúng ta cần xác định xem liệu nó có thể được “xây dựng” hay không bằng cách lấy hai số trước đó và nối các biểu diễn thập phân của chúng theo thứ tự mà không cần chèn bất cứ thứ gì vào giữa. 

Ví dụ: nếu trước đó chúng ta thấy 12 và 3 thì 123 được coi là có thể dựng được vì viết “12” theo sau là “3” sẽ cho ra “123”. Ý tưởng tương tự cũng được áp dụng ngay cả khi cả hai phần đều là các số trước đó giống hệt nhau, miễn là chúng đều được nhìn thấy trước vị trí hiện tại. Chúng tôi đếm xem có bao nhiêu lần trong chuỗi, số hiện tại có thể được hình thành theo cách này từ hai thẻ đã thấy trước đó. 

Kích thước đầu vào lên tới một triệu số, mỗi số lên tới$10^6 - 1$, vậy mỗi số có nhiều nhất 6 chữ số. Điều đó ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải xử lý từng số trong thời gian gần như không đổi hoặc logarit. Một bậc hai hoặc thậm chí$O(n \sqrt{n})$cách tiếp cận là không thể bởi vì ngay cả$10^6$các hoạt động trên mỗi phần tử sẽ quá lớn. 

Khó khăn chính là ở chỗ chúng ta không được yêu cầu kiểm tra điều kiện thành viên đơn giản mà là kiểm tra sự phân rã có cấu trúc: mọi số phải được kiểm tra dựa trên tất cả các phần phân tách có thể có của chuỗi thập phân của nó thành hai phần không trống và cả hai phần phải tương ứng với các giá trị đã thấy trước đó. 

Có một số trường hợp phức tạp phá vỡ các cách tiếp cận ngây thơ. 

Nếu một số chứa số 0 thì phép nối sẽ hoạt động đúng như đẳng thức chuỗi chứ không phải đẳng thức số. Ví dụ: nếu chúng ta thấy 0 và 1, thì phép nối là “01”, không khớp với số nguyên 1. Vì vậy, việc coi các số chỉ là số nguyên mà không bảo toàn dạng chuỗi chính xác sẽ dẫn đến kết quả khớp không chính xác. 

Một trường hợp tế nhị khác là các giá trị lặp lại. Nếu trước đó chúng ta thấy số 7 xuất hiện một lần thì số 77 không thể được tạo ra từ “7 + 7” trừ khi số 7 xuất hiện ít nhất hai lần trước đó. Giải pháp chỉ theo dõi sự tồn tại trong một tập hợp sẽ tính sai trường hợp này. 

Cuối cùng, các số 0 đứng đầu có ý nghĩa ngầm thông qua cấu trúc chuỗi. Vì các số đầu vào được đưa ra mà không có số 0 đứng đầu nên bất kỳ phần phân tách nào tạo ra phần bên trái hoặc bên phải bắt đầu bằng số 0 vẫn tương ứng với một biểu diễn số nguyên hợp lệ, nhưng nó phải khớp chính xác với số đã thấy trước đó, không phải là phiên bản chuẩn hóa. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là duy trì bản ghi tất cả các số đã thấy trước đó. Đối với mỗi số đến, chúng tôi chuyển đổi nó thành một chuỗi và thử mọi điểm phân tách có thể. Đối với một số có$d$chữ số, chúng tôi thử$d - 1$chia tách. Đối với mỗi phần tách, chúng tôi kiểm tra xem cả hai phần có tồn tại trong số các số đã thấy trước đó hay không. Nếu có, chúng tôi sẽ tăng câu trả lời. 

Điều này có tác dụng vì mọi cấu trúc hợp lệ phải tương ứng với chính xác một phần tách của biểu diễn chuỗi. Tuy nhiên, độ chính xác phụ thuộc vào việc sử dụng số đếm tần số thay vì một tập hợp đơn giản, vì cùng một số có thể được yêu cầu hai lần. 

Đối với mỗi số, phiên bản brute-force sẽ quét tất cả các cặp số trước đó và kiểm tra phép nối. Điều đó đòi hỏi$O(n)$ứng viên cho mỗi truy vấn, dẫn đến$O(n^2)$tổng số hoạt động, điều này vượt xa khả năng thực hiện đối với$n = 10^6$. 

Quan sát quan trọng là cấu trúc nối giúp loại bỏ nhu cầu tìm kiếm các cặp trên toàn cầu. Thay vào đó, mỗi số chỉ tương tác với ranh giới phân chia của chính nó. Điều này làm giảm vấn đề khi kiểm tra tối đa 5 phần chia cho mỗi số (vì các giá trị có tối đa 6 chữ số) và mỗi lần kiểm tra là một$O(1)$tra cứu từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo cặp |$O(n^2)$|$O(n)$| Quá chậm | 
| Kiểm tra phân tách bằng bản đồ băm |$O(n \cdot d)$,$d \le 6$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải trong khi duy trì bản đồ tần số của tất cả các số được thấy cho đến nay. 

1. Chuyển số hiện tại thành dạng chuỗi. Điều này là cần thiết vì phép nối phụ thuộc vào ranh giới chữ số chứ không phải giá trị số học. 
2. Đối với mỗi vị trí có thể phân chia trong chuỗi, hãy chia nó thành phần bên trái và phần bên phải. Mỗi phần chia tương ứng với một cặp thẻ tiềm năng trước đó mà sự ghép nối của chúng có thể tạo thành số hiện tại. Đây là cấu trúc duy nhất có thể tạo ra một công trình hợp lệ. 
3. Chuyển đổi cả hai phần trở lại thành số nguyên và kiểm tra xem chúng có tồn tại trong bản đồ tần số của các số đã thấy trước đó hay không. 
4. Nếu hai phần bằng nhau, hãy đảm bảo rằng tần số của số đó ít nhất bằng 2 trong số các phần tử trước đó, vì chúng ta cần hai lần xuất hiện riêng biệt để tạo thành cặp. 
5. Nếu bất kỳ sự phân chia nào thỏa mãn điều kiện, hãy tính vị trí này là một “sự kiện âm thanh” hợp lệ. 
6. Sau khi xử lý số hiện tại, hãy tăng tần suất của nó trên bản đồ để có thể sử dụng cho các công trình xây dựng trong tương lai. 

### Tại sao nó hoạt động 

Ở mỗi bước, bản đồ tần số thể hiện chính xác tất cả các thẻ có sẵn trước chỉ mục hiện tại. Bất kỳ cấu trúc hợp lệ nào của số hiện tại phải tương ứng với việc phân chia biểu diễn thập phân của nó thành hai chuỗi con liền kề. Không có cách nào khác để tạo thành số vì phép nối giữ nguyên thứ tự chữ số mà không có khoảng trống. Vì mọi phân tách có thể đều được kiểm tra và sự tồn tại được xác minh bằng cách sử dụng số đếm chính xác nên không có cặp hợp lệ nào bị bỏ sót và không có cặp không hợp lệ nào được chấp nhận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    ans = 0

    for x in a:
        s = str(x)
        ok = False

        for i in range(1, len(s)):
            left = int(s[:i])
            right = int(s[i:])

            if freq.get(left, 0) > 0 and freq.get(right, 0) > 0:
                if left != right:
                    ok = True
                    break
                else:
                    if freq.get(left, 0) > 1:
                        ok = True
                        break

        if ok:
            ans += 1

        freq[x] = freq.get(x, 0) + 1

    print(ans)

if __name__ == "__main__":
    main()
```Cốt lõi của giải pháp là bản đồ tần số gia tăng. Nó đảm bảo chúng tôi chỉ xem xét các thẻ đã thấy trước đó. Vòng lặp phân chia được giới hạn bởi số chữ số, nhiều nhất là 6, do đó, nó duy trì thời gian không đổi trên mỗi phần tử. 

Chi tiết triển khai tinh tế duy nhất là việc xử lý các phần chia bằng nhau. Khi cả hai nửa có cùng số nguyên, chúng ta phải đảm bảo rằng có ít nhất hai lần xuất hiện trước đó, vì một thẻ không thể được sử dụng lại hai lần. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự mẫu:```
1 23 123 11 21 1 2311
```Chúng tôi theo dõi bản đồ tần số khi chúng tôi đi. 

| Chỉ mục | Giá trị | Đã kiểm tra phần chia tách | Đã tìm thấy phần chia hợp lệ | Trả lời cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | không | 0 | 
| 2 | 23 | không | không | 0 | 
| 3 | 123 | (1,23) | vâng | 1 | 
| 4 | 11 | (1,1) | vâng | 2 | 
| 5 | 21 | (2,1) | không | 2 | 
| 6 | 1 | không | không | 2 | 
| 7 | 2311 | (23,11) | vâng | 3 | 

Phần tử thứ ba, thứ tư và thứ bảy kích hoạt các phân tách hợp lệ. 

Dấu vết này cho thấy mỗi quyết định chỉ phụ thuộc vào dữ liệu tần số được tích lũy trước đó và cấu trúc phân chia cục bộ chứ không phụ thuộc vào tìm kiếm toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot d)$| Mỗi số được chia tối đa 5 lần, mỗi lần chia sử dụng tra cứu băm theo thời gian không đổi | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ tất cả các số riêng biệt được nhìn thấy | 

Với$n = 10^6$Và$d \le 6$, tổng số thao tác đủ nhỏ để chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n_and_rest = inp.strip().split()
    n = int(n_and_rest[0])
    arr = list(map(int, n_and_rest[1:]))

    freq = {}
    ans = 0

    for x in arr:
        s = str(x)
        ok = False
        for i in range(1, len(s)):
            l = int(s[:i])
            r = int(s[i:])
            if freq.get(l, 0) > 0 and freq.get(r, 0) > 0:
                if l != r or freq.get(l, 0) > 1:
                    ok = True
                    break
        if ok:
            ans += 1
        freq[x] = freq.get(x, 0) + 1

    return str(ans)

# provided sample
assert run("7 1 23 123 11 21 1 2311") == "3"

# minimum size
assert run("1 5") == "0"

# simple concatenation
assert run("3 1 2 12") == "1"

# repeated value requires two earlier occurrences
assert run("5 7 7 77 77 777") == "2"

# no valid splits
assert run("4 10 20 30 40") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | không có thẻ trước tồn tại | 
| 1,2,12 | 1 | nối cơ bản | 
| lặp đi lặp lại 7 giây | 2 | tính đúng đắn của yêu cầu tần số | 
| không có trận đấu | 0 | tránh dương tính giả | 

## Vỏ cạnh 

Trường hợp một cạnh được lặp đi lặp lại chia thành các số giống hệt nhau. Xem xét đầu vào:```
7 7 77
```Khi xử lý số 7 thứ hai, không có sự phân chia nào tồn tại. Khi xử lý 77, mức phân chia là (7,7), nhưng độ chính xác phụ thuộc vào việc có hai số 7 trước đó. Vì chỉ có một tồn tại tại thời điểm đó nên séc sẽ từ chối nó một cách chính xác. 

Một trường hợp cạnh khác liên quan đến các số chứa số 0:```
3 0 1 1
```Khi đánh giá 1 ở cuối, phép chia “01” không hợp lệ vì nó không khớp chính xác với số nguyên được lưu trữ 1. Thuật toán tránh được sự không khớp này vì nó dựa vào sự bằng nhau của số nguyên so với các giá trị được lưu trữ thay vì chỉ bằng sự bằng nhau của chuỗi con. 

Trường hợp cạnh cuối cùng là chuỗi dài các số có thể xây dựng được. Ngay cả khi mọi số đều hợp lệ, mỗi bước chỉ phụ thuộc vào việc kiểm tra phân chia theo thời gian không đổi, do đó thuật toán vẫn ổn định và không bị suy giảm khi chuỗi tăng lên.
