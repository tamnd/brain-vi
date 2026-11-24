---
title: "CF 914B - Conan và Agasa chơi bài"
description: "Chúng tôi có một trò chơi trong đó Conan và Agasa lần lượt lấy các lá bài ra khỏi chồng bài. Mỗi thẻ có một số nguyên dương được viết trên đó. Khi người chơi chọn một lá bài, không chỉ lá bài đó bị loại bỏ mà tất cả các lá bài có số nhỏ hơn cũng bị loại bỏ."
date: "2026-06-12T10:02:13+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 914
codeforces_index: "B"
codeforces_contest_name: "Codecraft-18 and Codeforces Round 458 (Div. 1 + Div. 2, combined)"
rating: 1200
weight: 914
solve_time_s: 155
verified: true
draft: false
---

[CF 914B - Conan và Agasa chơi trò chơi bài](https://codeforces.com/problemset/problem/914/B) 

**Đánh giá:** 1200 
**Tags:** trò chơi, tham lam, thực hiện 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một trò chơi trong đó Conan và Agasa lần lượt lấy các lá bài ra khỏi chồng bài. Mỗi thẻ có một số nguyên dương được viết trên đó. Khi người chơi chọn một lá bài, không chỉ lá bài đó bị loại bỏ mà tất cả các lá bài có số nhỏ hơn cũng bị loại bỏ. Trò chơi kết thúc khi một người chơi không thể di chuyển vì không còn lá bài nào và người chơi đó thua cuộc. Conan luôn là người di chuyển trước. Nhiệm vụ là xác định xem ai sẽ thắng nếu cả hai đều chơi tối ưu. 

Đầu vào cho biết số lượng thẻ`n`và danh sách các số nguyên trên các thẻ đó. Đầu ra là "Conan" nếu người chơi đầu tiên có thể buộc phải thắng hoặc "Agasa" nếu người chơi thứ hai có thể. Những ràng buộc cho phép`n`lên tới 100.000 và mệnh giá thẻ lên tới 100.000. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại theo thời gian bậc hai trên các lá bài hoặc mô phỏng tất cả các bước di chuyển có thể xảy ra đều có khả năng là quá chậm. Chúng ta cần một giải pháp tuyến tính hoặc tuyến tính. 

Trường hợp cạnh phát sinh khi nhiều lá bài có cùng số. Ví dụ: nếu tất cả các thẻ giống hệt nhau và có số chẵn, Conan không thể loại bỏ tất cả các thẻ trong một lượt. Việc chọn bất kỳ lá bài nào chỉ loại bỏ những lá bài có giá trị đó. Sau đó Agasa có thể phản ánh các bước di chuyển cho đến khi hết thẻ. Một đầu vào nhỏ như`2\n5 5`nên xuất ra "Agasa", nhưng cách tiếp cận bất cẩn cho rằng lá bài lớn nhất luôn thắng sẽ trả về "Conan" không chính xác. Tương tự, nếu một lá bài là duy nhất và lớn hơn tất cả những lá bài khác, Conan có thể lấy nó và thắng ngay lập tức, như trong`3\n4 5 7`trong đó việc chọn 7 sẽ loại bỏ tất cả các thẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ mô phỏng mọi chuỗi hành động có thể xảy ra. Đến lượt Conan, thử từng lá bài, loại bỏ lá bài đã chọn và tất cả các lá bài nhỏ hơn, sau đó tính toán đệ quy các nước đi tối ưu của đối thủ. Điều này có thể đúng nhưng theo cấp số nhân trong`n`, vì mỗi lựa chọn thẻ có thể dẫn đến nhiều nhánh đệ quy. Ngay cả với tính năng ghi nhớ, không gian trạng thái vẫn rất lớn vì mỗi tập hợp con của các thẻ còn lại là một trạng thái riêng biệt, điều này khiến cho việc ghi nhớ không thể thực hiện được.`n = 10^5`. 

Điều quan trọng cần lưu ý là yếu tố duy nhất quan trọng là **sự chẵn lẻ về tần suất của mỗi số thẻ**. Mỗi nước đi sẽ loại bỏ tất cả các số nhỏ hơn, do đó trò chơi giảm thiểu một cách hiệu quả việc đếm số lượng thẻ tồn tại cho mỗi giá trị duy nhất. Nếu tồn tại bất kỳ số nào có **số lẻ**, Conan luôn có thể bắt đầu bằng cách lấy một trong số đó, để lại số chẵn hoặc buộc Agasa vào thế thua. Ngược lại, nếu tất cả các số đếm đều chẵn, Agasa có thể phản chiếu các bước di chuyển của Conan, cuối cùng khiến Conan không có lá bài nào trong lượt của mình. Điều này làm giảm vấn đề xuống việc kiểm tra tần suất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thẻ đầu vào`n`và danh sách giá trị thẻ. 
2. Đếm tần suất của mỗi giá trị thẻ bằng từ điển hoặc máy tính`Counter`. 
3. Lặp lại các giá trị tần số. 
4. Nếu tần số là số lẻ, hãy in "Conan" và dừng lại. Lý do là số lẻ cho phép Conan giành được lợi thế đầu tiên; bất cứ động thái nào Agasa thực hiện, Conan đều có thể đáp trả để duy trì sự cân bằng. 
5. Nếu tất cả các tần số đều là số chẵn, hãy in "Agasa". Trong trường hợp này, mọi hành động mà Conan thực hiện đều có thể bị Agasa phản chiếu cho đến khi lá bài cuối cùng được lấy ở lượt của Agasa. 
6. Kết thúc. 

Tại sao nó hoạt động: Tính bất biến là ở bất kỳ lượt nào, nếu tần số của số nhỏ nhất còn lại là số chẵn thì người chơi hiện tại không thể giành được lợi thế quyết định. Sự ngang bằng quyết định ai có thể giành chiến thắng. Bằng cách kiểm tra số lẻ, chúng tôi ngay lập tức xác định được người chơi nào có thể kiểm soát luồng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

n = int(input())
cards = list(map(int, input().split()))
freq = Counter(cards)

for count in freq.values():
    if count % 2 == 1:
        print("Conan")
        break
else:
    print("Agasa")
```Mã đọc số lượng thẻ và giá trị thẻ một cách hiệu quả. các`Counter`đếm số lần mỗi thẻ duy nhất xuất hiện. Vòng lặp kiểm tra bất kỳ số lẻ nào, điều này đảm bảo nước đi chiến thắng cho Conan. các`else`trên vòng lặp chỉ kích hoạt nếu không xảy ra ngắt, nghĩa là tất cả các số đếm đều là số chẵn. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3
4 5 7
```| Bước | tần số | Tìm thấy kỳ lạ? | Quyết định đầu ra | 
| --- | --- | --- | --- | 
| Khởi tạo | {4:1, 5:1, 7:1} | Thứ 1: 1 lẻ | In "Conan" | 

Ở đây, tất cả các số đếm đều là số lẻ, nhưng số lẻ đầu tiên được phát hiện cũng đủ để tuyên bố Conan thắng. Chọn lá bài 7 sẽ loại bỏ tất cả các lá bài. 

**Mẫu 2** 

đầu vào:```
4
2 2 3 3
```| Bước | tần số | Tìm thấy kỳ lạ? | Quyết định đầu ra | 
| --- | --- | --- | --- | 
| Khởi tạo | {2:2, 3:2} | 2 và 2 chẵn | Vòng lặp hoàn tất | 

Tất cả đều được tính chẵn. Bất kỳ lá bài nào Conan lấy đều có thể được phản chiếu bởi Agasa. Cuối cùng, Conan không có động thái nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tần số đọc và đếm tỷ lệ tuyến tính với số lượng thẻ | 
| Không gian | O(n) | Bộ đếm lưu trữ tần số cho mỗi giá trị thẻ duy nhất, nhiều nhất là`n`phím | 

Giải pháp dễ dàng phù hợp trong giới hạn cho`n = 10^5`và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    cards = list(map(int, input().split()))
    freq = Counter(cards)
    for count in freq.values():
        if count % 2 == 1:
            return "Conan"
    return "Agasa"

# Provided samples
assert run("3\n4 5 7\n") == "Conan", "sample 1"
assert run("4\n2 2 3 3\n") == "Agasa", "sample 2"

# Custom cases
assert run("1\n5\n") == "Conan", "single card"
assert run("2\n5 5\n") == "Agasa", "two identical cards"
assert run("5\n1 1 2 2 2\n") == "Conan", "mixed parity counts"
assert run("6\n1 1 2 2 3 3\n") == "Agasa", "all even counts"
assert run("7\n7 7 7 8 8 8 9\n") == "Conan", "complex parity mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n5 | Conan | Bài đơn, người chơi đầu tiên thắng | 
| 2\n5 5 | Agasa | Số lượng thẻ chẵn | 
| 5\n1 1 2 2 2 | Conan | Số chẵn lẻ hỗn hợp | 
| 6\n1 1 2 2 3 3 | Agasa | Tất cả đều được tính | 
| 7\n7 7 7 8 8 8 9 | Conan | Đếm số lẻ giữa nhiều số | 

## Vỏ cạnh 

Đối với một thẻ duy nhất`1\n5`, tần số là`{5:1}`, thật kỳ lạ. Thuật toán in "Conan" như mong đợi. Đối với hai thẻ giống hệt nhau`2\n5 5`, tần số là`{5:2}`, thậm chí. Vòng lặp không tìm thấy số lẻ nên "Agasa" được in. Đối với số lượng hỗn hợp như`5\n1 1 2 2 2`, tần số là`{1:2, 2:3}`. Lẻ đầu tiên (2:3) kích hoạt "Conan", hiển thị thuật toán phát hiện chính xác chiến lược chiến thắng. Việc triển khai xử lý nhất quán các đầu vào lớn và logic chẵn lẻ một cách chính xác.
