---
title: "CF 102277B - Tính chẵn lẻ của chuỗi"
description: "Nhiệm vụ là phân loại một chuỗi chữ thường theo tính chẵn lẻ về tần số của mỗi chữ cái. Một chuỗi được gọi ngay cả khi mỗi chữ cái xuất hiện với số lần chẵn. Nó được gọi là lẻ khi mỗi chữ cái viết thường xuất hiện với số lần lẻ."
date: "2026-08-16T19:32:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "B"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 58
verified: true
draft: false
---

[CF 102277B - Tính chẵn lẻ của chuỗi](https://codeforces.com/problemset/problem/102277/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là phân loại một chuỗi chữ thường theo tính chẵn lẻ về tần số của mỗi chữ cái. Một chuỗi được gọi ngay cả khi mỗi chữ cái xuất hiện với số lần chẵn. Nó được gọi là lẻ khi mỗi chữ cái viết thường xuất hiện với số lần lẻ. Nếu cả hai điều kiện đều không đúng thì câu trả lời là 2. Đầu vào chứa một chuỗi chữ thường không trống có độ dài tối đa là 70 và đầu ra là một số nguyên duy nhất, 0 cho chuỗi chẵn, 1 cho chuỗi lẻ và 2 nếu không. 

Giới hạn chiều dài nhỏ làm thay đổi đáng kể bức tranh thực tế. Ngay cả một giải pháp quét toàn bộ chuỗi một lần cho mỗi trong số 26 chữ cái viết thường cũng thực hiện tối đa (26 \times 70 = 1820) kiểm tra ký tự, điều này không đáng kể trong giới hạn một giây. Một lượt truyền duy nhất vẫn là giải pháp sạch hơn vì thông tin được yêu cầu chỉ là tần số chẵn lẻ của mỗi ký tự. 

Trường hợp cạnh đầu tiên là một chuỗi một ký tự. Ví dụ, đầu vào`a`nên sản xuất`1`, bởi vì`a`xảy ra một lần, trong khi mọi chữ cái viết thường khác không xảy ra lần nào. Số 0 là số chẵn và một là số lẻ, vì vậy mỗi chữ cái chỉ có tần số lẻ khi mỗi chữ cái trong bảng chữ cái xuất hiện chính xác với số lần lẻ. Điều này có nghĩa là kết quả đúng cho`a`thực sự là`2`, không`1`. Việc triển khai bất cẩn chỉ kiểm tra các chữ cái xuất hiện trong chuỗi sẽ phân loại sai chuỗi đó thành số lẻ. 

Trường hợp cạnh thứ hai là một chuỗi chứa mỗi chữ cái viết thường chính xác một lần. Ví dụ,`abcdefghijklmnopqrstuvwxyz`sản xuất`1`. Mỗi chữ cái trong số 26 chữ cái xuất hiện một lần, điều này thật kỳ lạ. Việc triển khai chỉ kiểm tra xem tất cả số đếm được quan sát có phải là số lẻ hay không có thể giải quyết đúng trường hợp này, trong khi việc triển khai quên rằng các chữ cái vắng mặt có tần số bằng 0 cũng sẽ chấp nhận không chính xác các chuỗi ngắn hơn như`a`. 

Trường hợp cạnh thứ ba là một chuỗi hoàn toàn bằng nhau với độ dài chẵn. Ví dụ,`aaaa`sản xuất`0`, bởi vì`a`xảy ra bốn lần và mọi chữ cái khác xảy ra không lần nào. Việc triển khai bất cẩn diễn giải "chẵn" chỉ đơn thuần là có độ dài chuỗi chẵn sẽ vượt qua ví dụ này, nhưng sẽ thất bại trong`aabbc`, có độ dài cũng là số lẻ trong khi các số chẵn lẻ về tần số chữ cái của nó xác định phân loại thực tế. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là xem xét từng chữ cái trong số 26 chữ cái viết thường một cách độc lập. Với mỗi chữ cái, hãy quét toàn bộ chuỗi và đếm xem chữ cái đó xuất hiện bao nhiêu lần. Sau khi đã biết tất cả các số đếm, hãy kiểm tra xem tất cả 26 số đếm có phải là số chẵn hay tất cả 26 số đếm đó là số lẻ hay không. Điều này đúng vì định nghĩa chỉ phụ thuộc vào tần số chẵn lẻ của mỗi chữ cái. Với độ dài chuỗi tối đa là 70, trường hợp xấu nhất thực hiện so sánh chính xác (26 \times 70 = 1820) ký tự, cộng với một lượng nhỏ sổ sách kế toán, do đó, nó đủ nhanh. Không có kích thước đầu vào nào khiến phương pháp ép buộc cụ thể này trở thành vấn đề theo các ràng buộc đã nêu. 

Cách tiếp cận rõ ràng hơn là tránh quét lại chuỗi. Duy trì 26 bộ đếm, một bộ đếm cho mỗi chữ cái viết thường và tăng bộ đếm tương ứng bất cứ khi nào một ký tự được đọc. Sau một lượt, kiểm tra tính chẵn lẻ của tất cả 26 bộ đếm. Nếu mọi bộ đếm đều chẵn thì câu trả lời là 0. Ngược lại, nếu mọi bộ đếm đều lẻ thì câu trả lời là 1. Nếu không có điều kiện nào đúng thì câu trả lời là 2. 

Quan sát quan trọng là tần số thực tế là không liên quan một khi tính chẵn lẻ của nó được biết đến. Chúng ta chỉ cần phân biệt 0, chẵn, lẻ thông qua tính chẵn lẻ của mỗi lần đếm. Vì bảng chữ cái được cố định ở 26 ký tự nên việc kiểm tra tất cả các bộ đếm là công việc liên tục. 

Brute-force hoạt động vì bảng chữ cái rất nhỏ nhưng nó liên tục kiểm tra các ký tự giống nhau. Quan sát thấy rằng tất cả các chữ cái có thể được đếm trong một lần truyền tải được chia sẻ sẽ loại bỏ sự lặp lại này và giảm việc xử lý chuỗi xuống một lần duyệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26n) = O(n) | O(26) = O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(26) = O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`cnt`với 26 mục, ban đầu tất cả đều bằng không. Lối vào`i`lưu trữ số lần xuất hiện của chữ cái viết thường có chỉ số bảng chữ cái là`i`. 
2. Duyệt qua từng ký tự`ch`trong chuỗi và tăng dần`cnt[ord(ch) - ord('a')]`. Bản đồ này`a`bởi vì`z`tới các chỉ số từ 0 đến 25 mà không cần từ điển. 
3. Kiểm tra xem mọi giá trị trong`cnt`là chẵn. Nếu có thì in`0`, bởi vì mỗi chữ cái viết thường xuất hiện một số lần chẵn. 
4. Nếu chuỗi không chẵn, hãy kiểm tra xem mọi giá trị trong`cnt`thật kỳ quặc. Nếu có thì in`1`, bởi vì mỗi chữ cái viết thường xuất hiện với số lần lẻ. 
5. Nếu không có điều kiện nào xảy ra, hãy in`2`. Ít nhất một chữ cái có tần số chẵn và ít nhất một chữ cái có tần số lẻ, do đó chuỗi không chẵn cũng không lẻ. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của chuỗi,`cnt[i]`chính xác là số lần xuất hiện của chữ cái tương ứng trong tiền tố đó. Sau khi toàn bộ chuỗi đã được xử lý, mảng sẽ chứa tần số chính xác của mỗi chữ cái viết thường. Hai phép kiểm tra tính chẵn lẻ cuối cùng khớp trực tiếp với định nghĩa của hai phân loại đặc biệt, vì vậy trường hợp còn lại phải là 2. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    cnt = [0] * 26

    for ch in s:
        cnt[ord(ch) - ord('a')] += 1

    if all(x % 2 == 0 for x in cnt):
        print(0)
    elif all(x % 2 == 1 for x in cnt):
        print(1)
    else:
        print(2)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đọc chuỗi duy nhất và xóa dòng mới ở cuối. Đầu vào được đảm bảo chứa các chữ cái viết thường, do đó không cần xác thực bổ sung. 

các`cnt`mảng tương ứng trực tiếp với 26 chữ cái. biểu hiện`ord(ch) - ord('a')`chuyển đổi`a`đến 0,`b`đến 1, v.v. cho đến hết`z`đến 25. Mỗi ký tự được xử lý chính xác một lần. 

đầu tiên`all`kiểm tra điều kiện chẵn cho toàn bộ bảng chữ cái, kể cả những chữ cái không xuất hiện. Một chữ cái vắng mặt có số 0 và số 0 là số chẵn nên những chữ cái đó phải được đưa vào bài kiểm tra. 

thứ hai`all`kiểm tra điều kiện lẻ. Đây là lý do tại sao một chuỗi như`a`không được phân loại là số lẻ: 25 chữ cái còn lại có số 0, số chẵn. 

Không có vấn đề tràn số nguyên trong Python và số lượng tối đa chỉ là 70. Đầu vào chứa một trường hợp kiểm thử duy nhất, do đó không cần vòng lặp trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, chuỗi là`coachessoaehwwwwww`. Số lượng tần số liên quan được hiển thị dưới đây. 

| Nhân vật | Đếm | Chẵn lẻ | 
| --- | --- | --- | 
| một | 1 | lẻ | 
| c | 1 | lẻ | 
| e | 2 | thậm chí | 
| h | 1 | lẻ | 
| o | 2 | thậm chí | 
| s | 2 | thậm chí | 
| w | 6 | thậm chí | 
| tất cả các chữ cái khác | 0 | thậm chí | 

Chuỗi chứa cả tần số lẻ và tần số chẵn, vì vậy nó không chẵn cũng không lẻ. Đầu ra là`2`. 

Đối với Mẫu 2, chuỗi là`coachesc`. 

| Nhân vật | Đếm | Chẵn lẻ | 
| --- | --- | --- | 
| một | 1 | lẻ | 
| c | 2 | thậm chí | 
| e | 2 | thậm chí | 
| h | 1 | lẻ | 
| o | 1 | lẻ | 
| s | 1 | lẻ | 
| tất cả các chữ cái khác | 0 | thậm chí | 

Một lần nữa, sự tương đương tần số được trộn lẫn. Đầu ra là`2`. 

Đầu vào mẫu được cung cấp khác`coachessoaehwwwwww`mỗi chữ cái đều được đếm chẵn, do đó nó tạo ra`0`. mẫu`coachesarefun`có tính chẵn lẻ hỗn hợp và tạo ra`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần, sau đó là kiểm tra kích thước không đổi trên 26 chữ cái. | 
| Không gian | O(1) | Mảng tần số luôn chứa chính xác 26 bộ đếm. | 

Với (n \le 70), thuật toán chỉ thực hiện vài chục thao tác ký tự và số lần kiểm tra tính chẵn lẻ không đổi. Nó thoải mái trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    s = input().strip()

    cnt = [0] * 26

    for ch in s:
        cnt[ord(ch) - ord('a')] += 1

    if all(x % 2 == 0 for x in cnt):
        print(0)
    elif all(x % 2 == 1 for x in cnt):
        print(1)
    else:
        print(2)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        old_stdout = sys.stdout
        sys.stdout = io.StringIO()
        solve()
        result = sys.stdout.getvalue()
        sys.stdout = old_stdout
        return result
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("coachessoaehwwwwww\n") == "0\n", "sample 1"
assert run("coachesarefun\n") == "2\n", "sample 2"
assert run("coachessoaehwwwwww\n") == "0\n", "sample 3"
assert run("coachesc\n") == "2\n", "sample 4"

# Minimum-size input
assert run("a\n") == "2\n", "single character"

# All 26 letters occur exactly once
assert run("abcdefghijklmnopqrstuvwxyz\n") == "1\n", "every letter once"

# All equal, even frequency
assert run("aaaa\n") == "0\n", "all equal with even count"

# Boundary length, all characters occur an even number of times
assert run("aabbccddeeffgghhiijjkkllmm\n") == "0\n", "26-letter boundary"

# Mixed parity
assert run("aab\n") == "2\n", "mixed frequency parity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`2`| Độ dài tối thiểu và yêu cầu xem xét các chữ cái vắng mặt | 
|`abcdefghijklmnopqrstuvwxyz`|`1`| Mỗi ký tự bảng chữ cái đều có tần số lẻ | 
|`aaaa`|`0`| Chuỗi hoàn toàn bằng nhau có tần số chẵn | 
|`aabbccddeeffgghhiijjkkllmm`|`0`| Tất cả 26 chữ cái đều có tần số chẵn | 
|`aab`|`2`| Hỗn hợp tần số chẵn và lẻ | 

## Vỏ cạnh 

Đối với đầu vào một ký tự`a`, thuật toán tạo ra số lượng với`a = 1`và mọi chữ cái khác đều bằng 0. Việc kiểm tra chẵn thất bại vì`a`có tần số lẻ. Thử nghiệm lẻ cũng thất bại vì 25 chữ cái còn lại có tần số bằng 0. Kết quả là`2`, điều này tránh được lỗi phổ biến là chỉ kiểm tra các ký tự xuất hiện. 

Đối với đầu vào`abcdefghijklmnopqrstuvwxyz`, mỗi bộ đếm chính xác là một. Bài kiểm tra chẵn không thành công đối với mọi chữ cái, trong khi bài kiểm tra số lẻ thành công đối với tất cả 26 bộ đếm. Kết quả là`1`. Điều này thực hiện bảng chữ cái đầy đủ và xác nhận rằng thuật toán không vô tình bỏ qua các chữ cái ở gần cuối bảng chữ cái. 

Vì`aaaa`, bộ đếm cho`a`trở thành bốn và mọi bộ đếm khác vẫn bằng không. Tất cả 26 số đều là số chẵn nên kết quả là`0`. Vụ việc cũng xác nhận rằng thuật toán kiểm tra tần số thay vì số lượng ký tự riêng biệt. 

Vì`aabbccddeeffgghhiijjkkllmm`, mỗi chữ cái viết thường xuất hiện đúng hai lần. Mọi bộ đếm đều là số chẵn, bao gồm tất cả các chữ cái được biểu thị rõ ràng trong đầu vào, do đó kết quả là`0`. Đây là một trường hợp ranh giới hữu ích vì nó chứa toàn bộ bảng chữ cái trong khi vẫn giữ nguyên mọi tần số. 

Vì`aab`, tần số là`a = 2`,`b = 1`, và tất cả các chữ cái còn lại bằng 0. Một số số là số chẵn và một số là số lẻ, do đó cả điều kiện tổng thể đều không giữ nguyên và thuật toán sẽ in ra`2`. Điều này phát hiện các triển khai quyết định sai câu trả lời từ tính chẵn lẻ của tổng chiều dài chuỗi thay vì tính chẵn lẻ của từng tần số chữ cái riêng lẻ.
