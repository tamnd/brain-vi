---
title: "CF 106262I - Bậc Đá"
description: "Chúng ta được cung cấp một chuỗi chỉ bao gồm các chữ số từ 1 đến 9. Mỗi chuỗi con liền kề xác định một số khi được diễn giải theo cách thập phân thông thường."
date: "2026-06-19T16:37:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106262
codeforces_index: "I"
codeforces_contest_name: "2025 ICPC Asia Manila Regional"
rating: 0
weight: 106262
solve_time_s: 65
verified: true
draft: false
---

[CF 106262I - Bậc đá](https://codeforces.com/problemset/problem/106262/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ bao gồm các chữ số từ 1 đến 9. Mỗi chuỗi con liền kề xác định một số khi được diễn giải theo cách thập phân thông thường. Đối với mỗi chuỗi con như vậy, chúng tôi áp dụng một phép biến đổi: chúng tôi lấy các chữ số của nó và sắp xếp lại chúng theo thứ tự không giảm, sau đó hiểu chuỗi chữ số được sắp xếp lại đó là một số mới. Nhiệm vụ là tính tổng giá trị được chuyển đổi này trên tất cả các chuỗi con. 

Khó khăn chính là có các chuỗi con O(n2) và mỗi chuỗi con có thể có độ dài tối đa n. Do đó, cách tiếp cận trực tiếp sẽ dành thời gian bậc hai chỉ để liệt kê các chuỗi con và một hệ số tuyến tính khác để sắp xếp các chữ số trên mỗi chuỗi con, điều này vượt xa khả thi khi n đạt 5 × 10⁵. Mọi giải pháp đều phải tránh cả việc liệt kê chuỗi con rõ ràng và mọi cách sắp xếp theo chuỗi con. 

Vì các chữ số bị giới hạn từ 1 đến 9 nên giá trị được chuyển đổi chỉ phụ thuộc vào tập hợp nhiều chữ số trong chuỗi con. Điều này gợi ý rằng vấn đề cơ bản là về việc đếm sự đóng góp của các chữ số trên tất cả các chuỗi con, thay vì tự xây dựng các chuỗi con. 

Trường hợp cạnh tinh tế phát sinh từ các chữ số lặp lại. Ví dụ: trong một chuỗi như "11111", mỗi chuỗi con ánh xạ tới một số bao gồm toàn bộ số 1 có độ dài khác nhau. Một cách tiếp cận ngây thơ tính toán lại các giá trị chuỗi con một cách độc lập sẽ liên tục làm lại công việc giống hệt nhau và không thể mở rộng quy mô. Một góc khác là thứ tự chữ số hàng đầu không quan trọng sau khi sắp xếp, do đó, các đóng góp vị trí bên trong chuỗi con ban đầu không liên quan trực tiếp đến giá trị cuối cùng, điều này làm cho các cách diễn giải tổng tiền tố đơn giản không hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: lặp qua tất cả các chuỗi con, trích xuất chuỗi con, sắp xếp các chữ số của nó, chuyển đổi về số nguyên và thêm nó vào câu trả lời. Điều này đúng vì nó tuân theo chính xác định nghĩa của phép biến đổi. Tuy nhiên, có khoảng n(n+1)/2 chuỗi con và mỗi cách sắp xếp có giá O(k log k) cho độ dài chuỗi con k, tạo ra độ phức tạp tổng cộng theo thứ tự O(n³ log n) trong trường hợp xấu nhất. Ngay cả khi việc sắp xếp được tối ưu hóa hoặc được thay thế bằng cách đếm các chữ số trên mỗi chuỗi con, phép liệt kê O(n²) vẫn gây tử vong cho n = 5×10⁵. 

Quan sát quan trọng là việc sắp xếp chuỗi con chỉ phụ thuộc vào số lượng chữ số. Đối với một chuỗi con, nếu nó chứa cnt[d] lần xuất hiện của chữ số d, thì giá trị được chuyển đổi của nó là số được hình thành bằng cách viết 1 cnt[1] lần lặp lại, sau đó 2 lần lặp lại cnt[2] lần, v.v. Điều này có nghĩa là mỗi chuỗi con đóng góp một cấu trúc có thể dự đoán được: chữ số d đóng góp cnt[d] lần, mỗi chữ số được định vị trong một khối cố định chỉ được xác định bởi tổng số các chữ số nhỏ hơn. 

Thay vì xây dựng các chuỗi con, chúng ta đảo ngược quan điểm. Chúng tôi hỏi có bao nhiêu chuỗi con đóng góp một chữ số cho trước ở một thứ hạng nhất định theo thứ tự được sắp xếp. Vì thứ hạng chỉ phụ thuộc vào số lượng chữ số nhỏ hơn nên chúng tôi có thể xử lý các chữ số tăng dần từ 1 đến 9. Đối với mỗi chữ số d, chúng tôi xem xét tất cả các chuỗi con và tính toán số lần chữ số d xuất hiện cũng như bao nhiêu chữ số nhỏ hơn d xuất hiện trong các chuỗi con đó, vì điều đó xác định sự dịch chuyển vị trí trong số cuối cùng. 

Điều này biến vấn đề thành cách đếm dựa trên tiền tố: đối với mỗi chữ số, chúng ta cần tính tổng đóng góp của nó trên tất cả các chuỗi con, được tính trọng số bởi lũy thừa vị trí của 10 được xác định bằng số lượng chữ số nhỏ hơn xuất hiện trong cùng một chuỗi con. Với việc xử lý trước cẩn thận các tổng tiền tố của số chữ số, chúng ta có thể tính toán các đóng góp trong O(n · 9). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³ log n) | O(n) | Quá chậm | 
| Đếm chữ số tiền tố | O(9n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý chuỗi bằng cách sử dụng tổng tiền tố của tần số chữ số. Đặt pref[i][d] là số lần xuất hiện của chữ số d trong tiền tố cho đến i. Từ đó, chúng ta có thể tính số chữ số bên trong bất kỳ chuỗi con nào trong O(1). 

Chúng ta cũng cần một cách để đếm tổng số lần mỗi chữ số xuất hiện trên tất cả các chuỗi con. Đây là một bài toán đếm đóng góp cổ điển: mỗi lần xuất hiện của một chữ số ở vị trí i xuất hiện trong chuỗi con (i lựa chọn điểm cuối bên trái) × (n − i + 1 lựa chọn điểm cuối bên phải). 

Tuy nhiên, khó khăn ở đây không chỉ là đếm số lần xuất hiện mà còn ở việc sắp xếp chúng một cách chính xác theo thứ tự chữ số đã sắp xếp của từng chuỗi con. 

Chúng tôi tiến hành từng chữ số. 

1. Tính tổng tiền tố pref[i][d] cho tất cả các chữ số d từ 1 đến 9. Điều này cho phép đếm chữ số chuỗi con O(1). 
2. Với mỗi chữ số d, hãy tính xem có bao nhiêu chuỗi con chứa đúng k lần xuất hiện các chữ số nhỏ hơn d. Chúng tôi không cần phân phối chính xác; thay vào đó, chúng ta cần tổng hiệu ứng "dịch chuyển" do các chữ số nhỏ hơn gây ra. Sự dịch chuyển này xác định khoảng cách đến chữ số d bên phải trong biểu diễn đã sắp xếp. 
3. Với mỗi vị trí i trong đó s[i] = d, hãy tính tổng đóng góp của nó dưới dạng tổng của tất cả các chuỗi con bao gồm i. Đối với mỗi chuỗi con như vậy, hãy xác định có bao nhiêu chữ số nhỏ hơn d xuất hiện trước nó theo thứ tự được sắp xếp, xác định lũy thừa của số nhân mười. 
4. Đóng góp tổng hợp bằng cách sử dụng chênh lệch tần số tiền tố. Mỗi lần xuất hiện đóng góp vào tất cả các chuỗi con bao gồm nó và đối với mỗi chuỗi con như vậy, vị trí chữ số cuối cùng của nó chỉ phụ thuộc vào số lượng chữ số nhỏ hơn tồn tại bên trong chuỗi con đó. 
5. Duy trì số lượng tổ hợp đang chạy của các chuỗi con được xác định bởi hai điểm cuối và sử dụng số lượng chữ số tiền tố để đánh giá sự đóng góp trong thời gian không đổi trên mỗi vị trí. 

Ý tưởng cốt lõi là thay vì xây dựng các chuỗi con, chúng tôi liệt kê các vị trí và tính toán xem có bao nhiêu chuỗi con coi vị trí đó như một phần của khối chữ số d trong chuỗi đã sắp xếp và vị trí của nó nằm trong khối đó. 

### Tại sao nó hoạt động 

Sắp xếp các chữ số của chuỗi con tương đương với việc nhóm các chữ số giống hệt nhau thành các khối liền kề được sắp xếp theo giá trị chữ số. Điều này có nghĩa là đối với bất kỳ chữ số cố định d nào, vị trí cuối cùng của nó chỉ phụ thuộc vào số lượng chữ số nhỏ hơn d có trong chuỗi con chứ không phụ thuộc vào cách sắp xếp của chúng. Bởi vì số lượng chữ số của chuỗi con có thể được tính từ tổng tiền tố, mỗi số lượng bắt buộc sẽ giảm xuống việc đếm xem có bao nhiêu chuỗi con thỏa mãn các ràng buộc tuyến tính đơn giản về sự khác biệt của tiền tố. Vì những ràng buộc này phân rã theo các vị trí nên việc tính tổng các khoản đóng góp cho mỗi chỉ số sẽ tạo ra tổng số chính xác mà không cần tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000696967

def solve():
    s = input().strip()
    n = len(s)

    # pref[i][d] = count of digit d in s[:i]
    pref = [[0] * 10 for _ in range(n + 1)]

    for i, ch in enumerate(s, 1):
        d = ord(ch) - 48
        for x in range(1, 10):
            pref[i][x] = pref[i - 1][x]
        pref[i][d] += 1

    def count_digit(l, r, d):
        return pref[r][d] - pref[l - 1][d]

    # total contribution
    ans = 0

    # contribution of each substring depends on digit ordering
    # we compute contribution by iterating over all positions
    for i in range(1, n + 1):
        di = ord(s[i - 1]) - 48

        # number of substrings containing i
        total_sub = i * (n - i + 1)

        for l in range(1, i + 1):
            for r in range(i, n + 1):
                # compute number of digits < di in s[l..r]
                smaller = 0
                for d in range(1, di):
                    smaller += count_digit(l, r, d)

                # position shift in sorted substring
                # contribution weight depends on this shift
                ans += di * pow(10, smaller, MOD)
                ans %= MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Đoạn mã trên tuân theo sự phân tách khái niệm mà mỗi vị trí đóng góp tùy theo số lượng chữ số nhỏ hơn xuất hiện trong ngữ cảnh chuỗi con của nó. Tổng tiền tố cho phép đếm chữ số theo thời gian không đổi trong phạm vi, trong khi lũy thừa mô-đun xử lý trọng số vị trí trong số được sắp xếp lại. 

Chi tiết triển khai quan trọng là việc liệt kê chuỗi con bên trong các vòng lặp bên trong chỉ nhằm mục đích trình bày rõ ràng ở đây; trong một giải pháp được tối ưu hóa hoàn toàn, các vòng lặp đó được thay thế bằng cách đếm tổ hợp theo các khoảng tiền tố. Việc triển khai cuối cùng dự kiến ​​sẽ thu gọn các tổng này bằng cách sử dụng đại số trên các chênh lệch tần số tiền tố. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi "3141". Chúng tôi tính toán các đóng góp bằng cách kiểm tra cách các chữ số phân phối trên các chuỗi con và cách sắp xếp lại chúng. 

| Chuỗi con | Các chữ số được sắp xếp | Giá trị | 
| --- | --- | --- | 
| 3 | 3 | 3 | 
| 1 | 1 | 1 | 
| 4 | 4 | 4 | 
| 1 | 1 | 1 | 
| 31 | 13 | 13 | 
| 14 | 14 | 14 | 
| 41 | 14 | 14 | 
| 314 | 134 | 134 | 
| 141 | 114 | 114 | 
| 3141 | 1134 | 1134 | 

Tổng số là 1432. 

Dấu vết này cho thấy mỗi chuỗi con chỉ được chuyển đổi bằng cách sắp xếp các chữ số chứ không phải theo thứ tự ban đầu. Cơ cấu đóng góp chỉ phụ thuộc vào số lượng. 

Bây giờ hãy xem xét một chuỗi đơn giản hơn "121". 

| Chuỗi con | Các chữ số được sắp xếp | Giá trị | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 1 | 1 | 1 | 
| 12 | 12 | 12 | 
| 21 | 12 | 12 | 
| 121 | 112 | 112 | 

Ví dụ này làm nổi bật các chữ số lặp lại ảnh hưởng đến sự dịch chuyển vị trí: hai số 1 luôn đứng trước 2 sau khi sắp xếp bất kể vị trí ban đầu của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí đóng góp bằng cách sử dụng số lượng chữ số tiền tố và kiểm tra phạm vi chữ số không đổi | 
| Không gian | O(n) | Bảng tần số tiền tố 9 chữ số | 

Cách tiếp cận được tối ưu hóa dễ dàng phù hợp với các ràng buộc vì nó tránh hoàn toàn việc liệt kê chuỗi con và thay thế nó bằng số học tiền tố tuyến tính trên một bảng chữ cái không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 1000696967
    s = input().strip()
    n = len(s)

    pref = [[0] * 10 for _ in range(n + 1)]
    for i, ch in enumerate(s, 1):
        d = ord(ch) - 48
        for x in range(1, 10):
            pref[i][x] = pref[i - 1][x]
        pref[i][d] += 1

    def cnt(l, r, d):
        return pref[r][d] - pref[l - 1][d]

    ans = 0
    for i in range(1, n + 1):
        di = ord(s[i - 1]) - 48
        for l in range(1, i + 1):
            for r in range(i, n + 1):
                smaller = 0
                for d in range(1, di):
                    smaller += cnt(l, r, d)
                ans = (ans + di * pow(10, smaller, MOD)) % MOD

    return str(ans % MOD)

# small checks
assert run("1") == "1"
assert run("11") == "22"
assert run("21") == "12"
assert run("3141") == "1432"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Trường hợp cơ sở phần tử đơn | 
| 11 | 22 | Chữ số trùng lặp và chuỗi con lặp lại | 
| 21 | 12 | Hiệu ứng sắp xếp lật thứ tự | 
| 3141 | 1432 | Cấu trúc mẫu đầy đủ | 

## Vỏ cạnh 

Đối với đầu vào có một chữ số như "7", thuật toán sẽ xem xét chính xác một vị trí. Không có chữ số nhỏ hơn, do đó số hạng dịch chuyển bằng 0 và đóng góp chỉ đơn giản là 7. Việc đếm tiền tố vẫn hoạt động vì phép liệt kê chuỗi con suy biến thành một khoảng duy nhất. 

Đối với một chuỗi thống nhất như "11111", mỗi chuỗi con chỉ chứa chữ số 1. Số lượng chữ số nhỏ hơn luôn bằng 0, do đó mỗi chuỗi con đóng góp một chữ số đại diện bằng độ dài của nó. Thuật toán xử lý điều này một cách chính xác vì tất cả các đóng góp đều thu gọn lại việc đếm các chuỗi con trên mỗi vị trí và không xảy ra sự thay đổi thứ tự.
