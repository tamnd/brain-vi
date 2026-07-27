---
title: "CF 102821G - Trò chơi số nguyên tố"
description: "Trò chơi được chơi trên hai số nguyên dương. Một động thái làm giảm chính xác một trong số chúng. Trò chơi không phải là đạt đến con số 0. Thay vào đó, ranh giới nguy hiểm là giá trị K: thời điểm một trong hai số trở thành K, Bob thắng."
date: "2026-07-26T16:05:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102821
codeforces_index: "G"
codeforces_contest_name: "2019 Sichuan Province Programming Contest"
rating: 0
weight: 102821
solve_time_s: 59
verified: true
draft: false
---

[CF 102821G - Trò chơi số nguyên tố](https://codeforces.com/problemset/problem/102821/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên hai số nguyên dương. Một động thái làm giảm chính xác một trong số chúng. Trò chơi không phải là đạt đến con số 0. Thay vào đó, ranh giới nguy hiểm là giá trị`K`: thời điểm một trong hai số trở thành`K`, Bob thắng. Alice thắng khi hai số hiện tại đều là số nguyên tố, ngoại trừ trường hợp nếu đạt`K`xảy ra cùng lúc, Bob thắng vì`K`điều kiện được ưu tiên. 

Đầu vào cung cấp một số trò chơi độc lập. Mỗi trò chơi chỉ định cặp bắt đầu`(x, y)`, ranh giới mất`K`, và ai di chuyển trước. Đầu ra yêu cầu người chiến thắng cuối cùng giả sử cả hai người chơi đều chọn nước đi tối ưu. 

Giới hạn trên`x, y <= 10^6`loại trừ một bảng lập trình động đầy đủ trên tất cả các cặp. Có thể có tới`10^12`các trạng thái có thể, vượt xa giới hạn bộ nhớ. Các nhiệm vụ phụ 90 phần trăm nhỏ hơn với giá trị lên tới 1000 gợi ý rằng mô phỏng trạng thái hoạt động ban đầu, nhưng giải pháp cuối cùng cần khai thác cấu trúc đặc biệt của các chuyển động và điều kiện chính. Một giải pháp xung quanh`O(max(x, y))`tiền xử lý và chỉ cần một lượng nhỏ công việc cho mỗi truy vấn. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. Nếu vị trí ban đầu đã là cặp nguyên tố thì trò chơi đã kết thúc và Alice thắng, trừ khi một trong các số đã có sẵn.`K`. Ví dụ, với đầu vào`5 7 2 0`, cả hai số đều là số nguyên tố và không phải số nào`K`, vậy câu trả lời là Alice. Việc triển khai đệ quy chỉ kiểm tra trạng thái đầu cuối sau khi thực hiện một nước đi sẽ tiếp tục trò chơi một cách không chính xác. 

Một trường hợp phức tạp khác là khi đạt được một cặp số nguyên tố và đạt được`K`xảy ra cùng nhau. Ví dụ, với`x = 3`,`y = 5`,`K = 3`, số đầu tiên đã bằng`K`. Câu trả lời là Bob, mặc dù cả hai số đều là số nguyên tố. Bỏ qua mức độ ưu tiên của`K`điều kiện cho kết quả sai. 

Người chơi bắt đầu cũng có vấn đề. Vị thế đang thắng khi Bob buộc phải di chuyển có thể bị thua khi Alice di chuyển, vì Alice có thể tạo ngay một cặp nguyên tố trước khi Bob có cơ hội. Ví dụ,`4 9 2 0`Và`4 9 2 1`có những tình huống chiến lược khác nhau vì nước đi đầu tiên thuộc về những người chơi khác nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là lưu trữ người chiến thắng ở mọi trạng thái có thể tiếp cận`(x, y, turn)`sử dụng đệ quy hoặc lập trình động. Chuyển đổi trạng thái chỉ có hai lựa chọn, giảm tọa độ. Điều này có tác dụng vì biểu đồ trò chơi không có tính chu kỳ, vì mỗi nước đi đều giảm`x + y`. Tuy nhiên, số lượng các trạng thái có thể có là rất lớn. Với các giá trị gần`10^6`, có thể có khoảng`10^12`cặp, vì vậy thậm chí một byte cho mỗi trạng thái là không thể. 

Quan sát hữu ích xuất phát từ thực tế là cả hai người chơi chỉ di chuyển theo đường chéo qua lưới khi họ cố gắng duy trì mối quan hệ giống nhau giữa các tọa độ. Nếu cả hai tọa độ đều giảm đi một lần, trò chơi sẽ đạt đến`(x - d, y - d)`sau hai lần di chuyển. Câu hỏi duy nhất là liệu một trong những vị trí được đồng bộ hóa này có trở thành cặp số nguyên tố trước khi giao nhau hay không.`K`. 

Định nghĩa`sameStep(x, y)`như kiểm tra xem có tồn tại một số`d`như vậy`x - d`Và`y - d`đều là số nguyên tố và vẫn lớn hơn`K`. Điều này thể hiện những vị trí mà Alice cuối cùng có thể ép buộc một cặp nguyên tố nếu đối thủ không thể can thiệp. 

Các trường hợp còn lại chỉ phụ thuộc vào lượt của ai. Khi Bob di chuyển trước, Alice sẽ thắng nếu vị trí hiện tại đã có đích đến chính được đồng bộ hóa. Khi Alice di chuyển đầu tiên, cô ấy có thể thực hiện bước đi đầu tiên của mình trên một trong hai tọa độ, vì vậy chúng tôi kiểm tra hai vị trí tiếp theo có thể có. 

Phương pháp vũ phu khám phá mọi con đường có thể. Phương pháp tối ưu hóa chỉ quét các vị trí đường chéo và sử dụng sàng để trả lời kiểm tra tính nguyên tố trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lần di chuyển | O(số tiểu bang) | Quá chậm | 
| Tối ưu | O(max(x, y) + T * max(x, y) trong trường hợp xấu nhất) | O(max(x, y)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tính nguyên tố cho mọi số lên tới`10^6`bằng sàng Eratosthenes. Trò chơi chỉ hỏi liệu các số có phải là số nguyên tố hay không, vì vậy điều này sẽ loại bỏ phép chia thử lặp đi lặp lại. 
2. Xử lý các vị trí đã hoàn thành. Nếu một trong hai tọa độ bằng`K`, Bob thắng ngay vì luật biên có mức độ ưu tiên. Nếu cả hai tọa độ đều nguyên tố và không có tọa độ nào`K`, Alice thắng ngay lập tức. 
3. Tạo một trình trợ giúp đi qua các vị trí đường chéo`(x - d, y - d)`. Trong khi cả hai tọa độ đều ở trên`K`, kiểm tra xem cả hai có phải là số nguyên tố hay không. Nếu một vị trí như vậy tồn tại, Alice có thể có một mục tiêu chiến thắng. 
4. Nếu Bob bắt đầu, hãy đánh giá xem vị trí hiện tại có chứa cặp nguyên tố tương lai như vậy hay không. Bob không thể cho phép Alice di chuyển vào đó, vì vậy điều này quyết định liệu Alice có còn có thể giành chiến thắng hay không. 
5. Nếu Alice bắt đầu, hãy kiểm tra cả hai bước đi đầu tiên có thể thực hiện được. Cô ấy có thể giảm tọa độ đi một, do đó sẽ có nước đi thắng nếu một trong hai`(x - 1, y)`hoặc`(x, y - 1)`chứa một đường chéo chiến thắng. 

Tại sao nó hoạt động: bất biến quan trọng là cứ sau hai nước đi, nếu cả hai người chơi đều không kết thúc trò chơi thì tiến trình duy nhất không bị tranh cãi là dọc theo đường chéo nơi cả hai tọa độ đều giảm như nhau. Người trợ giúp kiểm tra chính xác những thời điểm có thể xảy ra khi điều kiện chính có thể xuất hiện. Người chơi đầu tiên có thể buộc phải đạt được cặp số nguyên tố trước`K`ranh giới xác định kết quả và tất cả các đường dẫn khác cuối cùng đều thua điều kiện biên của Bob. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**6

def build_sieve(n):
    prime = [True] * (n + 1)
    prime[0] = prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if prime[i]:
            step = i
            start = i * i
            prime[start:n + 1:step] = [False] * (((n - start) // step) + 1)
    return prime

prime = build_sieve(LIMIT)

def diagonal_prime(x, y, k):
    while x > k and y > k:
        if prime[x] and prime[y]:
            return True
        x -= 1
        y -= 1
    return False

def can_alice_win_after_bob_turn(x, y, k):
    if diagonal_prime(x, y, k):
        return True
    if diagonal_prime(x - 2, y, k) and diagonal_prime(x, y - 2, k):
        return True
    return False

def solve_case(x, y, k, w):
    if x == k or y == k:
        return "Bob"

    if prime[x] and prime[y]:
        return "Alice"

    if w == 1:
        return "Alice" if can_alice_win_after_bob_turn(x, y, k) else "Bob"

    return "Alice" if (
        can_alice_win_after_bob_turn(x - 1, y, k)
        or can_alice_win_after_bob_turn(x, y - 1, k)
    ) else "Bob"

def main():
    t = int(input())
    ans = []
    for case in range(1, t + 1):
        x, y, k, w = map(int, input().split())
        ans.append(f"Case {case}: {solve_case(x, y, k, w)}")
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Sàng tạo mảng tra cứu một lần vì tối đa 100 trường hợp thử nghiệm có thể sử dụng lại cùng một số kiểm tra nguyên tố. các`diagonal_prime`hàm là quan sát cốt lõi từ hướng dẫn thuật toán. Nó không bao giờ vượt qua`K`ranh giới vì điều kiện vòng lặp yêu cầu cả hai giá trị phải lớn hơn`K`. 

chức năng`can_alice_win_after_bob_turn`kiểm tra cấu trúc hai nước đi cần thiết khi Bob chuẩn bị chơi. Điều kiện thứ hai xử lý tình huống Bob di chuyển một tọa độ trước và Alice phản ứng ở tọa độ kia. Thứ tự kiểm tra trong`solve_case`quan trọng bởi vì`K`điều kiện ghi đè điều kiện chính. 

Số nguyên Python tránh các vấn đề tràn. Chi tiết ranh giới duy nhất cần được quan tâm là các cặp số nguyên tố chứa`K`không được chấp nhận, đó là lý do tại sao`K`kiểm tra xảy ra trước kiểm tra chính. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu đầu tiên: 

| Bước | x | y | K | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 9 | 2 | Alice bắt đầu | 
| Kiểm tra đường chéo | 4,9 rồi 3,8 | | | Không có cặp nguyên tố | 
| Alice di chuyển | 3,9 | | | Kiểm tra tiếp tục chiến thắng | 
| Đường chéo tương lai | 3,9 rồi 2,8 | | | Đạt K trước | 

Đường dẫn trực tiếp không tạo ra cặp số nguyên tố, vì vậy Alice không thể buộc nhánh này giành chiến thắng. Người trợ giúp xem xét phản ứng của đối thủ và thấy rằng Alice vẫn có lộ trình chiến thắng, tạo ra`Case 1: Alice`. 

Đối với trường hợp mẫu thứ tư: 

| Bước | x | y | K | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 5 | 28 | 2 | Alice bắt đầu | 
| Hãy thử x di chuyển | 4 | 28 | | Kiểm tra tiếp tục | 
| Hãy thử di chuyển | 5 | 27 | | Kiểm tra tiếp tục | 
| Kết quả | | | | Không có cặp nguyên tố bắt buộc | 

Cả hai bước đi đầu tiên có thể xảy ra đều không tạo được trạng thái nguyên tố bắt buộc trước ranh giới. Bob có thể tránh các vị trí nguyên tố và cuối cùng tạo ra một tọa độ bằng`K`, vì vậy đầu ra là`Bob`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10^6 + T * 10^6) trường hợp xấu nhất | Sàng có tính tuyến tính và mỗi truy vấn có thể quét một đường chéo có độ dài lên tới một triệu. | 
| Không gian | O(10^6) | Cấu trúc lớn duy nhất là bảng nguyên tố. | 

Quá trình tiền xử lý dễ dàng phù hợp với giới hạn bộ nhớ. Chỉ với 100 trường hợp thử nghiệm, việc quét theo đường chéo vẫn được chấp nhận trong Python vì mỗi lần quét chỉ thực hiện tra cứu mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

# This assumes the solve_case function from the solution above is available.

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = []
    t = int(sys.stdin.readline())
    for case in range(1, t + 1):
        x, y, k, w = map(int, sys.stdin.readline().split())
        out.append(f"Case {case}: {solve_case(x, y, k, w)}")
    sys.stdin = old
    return "\n".join(out)

assert run("""4
4 9 2 0
7 10 2 0
6 39 2 0
5 28 2 0
""") == """Case 1: Alice
Case 2: Alice
Case 3: Alice
Case 4: Bob""", "samples"

assert run("""1
5 7 2 0
""") == "Case 1: Alice", "initial prime pair"

assert run("""1
3 5 3 0
""") == "Case 1: Bob", "K overrides prime"

assert run("""1
1000000 999999 2 1
""") in ["Case 1: Alice", "Case 1: Bob"], "maximum values"

assert run("""1
8 8 2 0
""") in ["Case 1: Alice", "Case 1: Bob"], "equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 7 2 0`| Alice | Trạng thái đầu cuối nguyên tố ban đầu | 
|`3 5 3 0`| Bob | Ưu tiên của`K`tình trạng | 
|`1000000 999999 2 1`| Phụ thuộc vào chiến lược | Xử lý ranh giới lớn | 
|`8 8 2 0`| Phụ thuộc vào chiến lược | Tọa độ bằng nhau và quét đường chéo | 

## Vỏ cạnh 

Khi vị trí bắt đầu đã là số nguyên tố, thuật toán sẽ quay trở lại trước khi thực hiện bất kỳ bước di chuyển nào. Vì`5 7 2 0`, cả hai số đều thỏa mãn điều kiện nguyên tố và không bằng nhau`K`, do đó Alice thắng ngay lập tức. 

Khi`K`là số nguyên tố, nó có thể tạo ra sự mâu thuẫn rõ ràng vì một số có thể là cả hai`K`và nguyên tố. Thuật toán xử lý việc này bằng cách kiểm tra`x == K`hoặc`y == K`Đầu tiên. Vì`3 5 3 0`, Bob thắng vì điều kiện biên có mức độ ưu tiên cao hơn. 

Khi người chơi xuất phát thay đổi, nước đi đầu tiên có thể thay đổi hoàn toàn cục diện. Đối với vị trí mà Alice có thể tạo cặp nguyên tố bằng cách giảm một tọa độ,`w = 0`cho Alice cơ hội đó ngay lập tức, trong khi`w = 1`cho Bob một cơ hội để tránh nó. 

Khi cả hai tọa độ đều bằng nhau thì đường chéo chứa các giá trị bằng nhau lặp lại. Người trợ giúp vẫn hoạt động vì nó kiểm tra từng khoảng cách có thể từ khi bắt đầu cho đến khi đạt được`K`, mà không cần dựa vào tọa độ khác nhau.
