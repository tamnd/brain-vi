---
title: "CF 1097A - Gennady và trò chơi bài"
description: "Trò chơi Gennady chơi liên quan đến việc kết hợp các thẻ theo thứ hạng hoặc chất. Về mặt thực tế, bạn được cấp một lá bài duy nhất trên bàn và một tay bài gồm năm lá bài."
date: "2026-06-12T05:45:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1097
codeforces_index: "A"
codeforces_contest_name: "Hello 2019"
rating: 800
weight: 1097
solve_time_s: 77
verified: true
draft: false
---

[CF 1097A - Gennady và trò chơi bài](https://codeforces.com/problemset/problem/1097/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi Gennady chơi liên quan đến việc kết hợp các thẻ theo thứ hạng hoặc chất. Về mặt thực tế, bạn được cấp một lá bài duy nhất trên bàn và một tay bài gồm năm lá bài. Mỗi lá bài được biểu thị bằng hai ký tự: ký tự đầu tiên là cấp bậc, có thể là số từ 2 đến 9 hoặc một chữ cái cho T, J, Q, K, A. Ký tự thứ hai là bộ, là một trong các D, C, S hoặc H. Nhiệm vụ là xác định xem có ít nhất một lá bài trên tay bạn có cùng thứ hạng hoặc chất với lá bài trên bàn hay không. Nếu có một thẻ như vậy, bạn có thể chơi nó và đầu ra phải là "CÓ". Nếu không, đầu ra sẽ là "KHÔNG". 

Những hạn chế là nhỏ. Bạn chỉ có một thẻ bàn và năm thẻ tay. Điều này có nghĩa là bất kỳ thuật toán nào kiểm tra từng thẻ riêng lẻ đều đủ nhanh. Không cần cấu trúc dữ liệu nâng cao, băm hoặc sắp xếp. Vấn đề về cơ bản là thời gian không đổi do kích thước đầu vào cố định và thách thức chính là việc triển khai cẩn thận hơn là hiệu quả thuật toán. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó ván bài chứa các quân bài chỉ khớp với quân bài trên bàn theo chất hoặc chỉ theo cấp bậc. Ví dụ: nếu quân bài trên bàn là 9H và tay bài của bạn là 2H 3D 4S 5C 6D thì bạn có thể chơi 2H vì nó hợp với chất. Một trường hợp tinh tế khác là có nhiều lá bài trong tay có thể chơi được; bạn vẫn chỉ cần xuất "CÓ" một lần và không quan trọng đó là thẻ nào. Việc triển khai bất cẩn có thể yêu cầu cả cấp bậc và bộ đồ phải khớp không chính xác, điều này sẽ không thành công trong các ví dụ này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là dùng vũ lực: lặp lại từng lá bài trong tay bạn và kiểm tra xem thứ hạng của nó có khớp với thứ hạng của quân bài trên bàn hay chất của nó khớp với chất của quân bài trên bàn hay không. Vì chỉ có năm thẻ nên đây thực sự là một số thao tác không đổi. Cách tiếp cận bạo lực được đảm bảo chính xác vì nó kiểm tra toàn diện tất cả các khả năng và thời gian chạy của nó không đáng kể đối với đầu vào nhất định. 

Không có cách tiếp cận nào nhanh hơn về độ phức tạp tiệm cận vì phương pháp vũ phu đã kiểm tra tất cả các thẻ một lần. Quan sát cho phép đưa ra quyết định ngay lập tức là bạn có thể trả lại "CÓ" ngay khi tìm thấy thẻ phù hợp. Không cần phải kiểm tra các quân bài còn lại sau khi phát hiện được một nước đi hợp lệ. Việc thoát sớm nhỏ này chỉ là một tối ưu hóa nhỏ nhưng làm rõ logic: khi có kết quả khớp, bạn đã hoàn thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(5) → O(1) hiệu quả | O(1) | Đã chấp nhận | 
| Tối ưu | O(5) → O(1) hiệu quả | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc quân bài trên bàn và quân bài năm tay. Ký tự đầu tiên của mỗi chuỗi là cấp bậc, ký tự thứ hai là chất. 
2. Trích xuất cấp bậc và chất từ ​​thẻ bàn thành các biến riêng biệt cho rõ ràng. 
3. Lặp lại từng lá bài trên tay bạn. 
4. Đối với mỗi lá bài trên tay, hãy trích xuất cấp bậc và chất của nó. 
5. Kiểm tra xem thứ hạng có khớp với thứ hạng của lá bài trong bàn hay chất có khớp với chất của lá bài trên bàn hay không. 
6. Nếu tìm thấy kết quả phù hợp, hãy in ngay "CÓ" và dừng chương trình. 
7. Nếu vòng lặp hoàn tất mà không tìm thấy kết quả nào phù hợp, hãy in "NO". 

Thuật toán này hoạt động vì tính bất biến rất đơn giản: chúng ta chỉ được phép chơi một quân bài nếu nó phù hợp với thứ hạng hoặc chất của quân bài trên bàn. Bằng cách kiểm tra từng lá bài riêng lẻ và quay lại ngay sau khi khớp thành công, chúng tôi đảm bảo rằng không có lá bài hợp lệ nào bị bỏ qua và không có lá bài không hợp lệ nào kích hoạt "CÓ". 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

table_card = input().strip()
hand_cards = input().strip().split()

table_rank = table_card[0]
table_suit = table_card[1]

for card in hand_cards:
    if card[0] == table_rank or card[1] == table_suit:
        print("YES")
        break
else:
    print("NO")
```Đầu tiên, mã sẽ đọc thẻ trên bàn và chia tay thành danh sách. Nó lưu trữ rõ ràng thứ hạng và chất của thẻ trên bàn, điều này tránh việc lập chỉ mục lặp lại. Vòng lặp lần lượt kiểm tra từng lá bài trên tay. Sử dụng`else`mệnh đề trên vòng lặp for đảm bảo rằng "KHÔNG" chỉ được in nếu không có thẻ nào khớp, một cách tiếp cận Pythonic tinh tế nhưng rõ ràng. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
AS
2H 4C TH JH AD
```| Bước | Thẻ | Trận đấu xếp hạng? | Phù hợp với trận đấu? | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2H | Không | Có (H) | In CÓ, dừng | 

Điều này cho thấy việc so khớp chất là đủ, xác nhận thuật toán xác định chính xác các quân bài có thể chơi được. 

**Mẫu 2** 

đầu vào:```
4D
5H 6S 7C 8H 9S
```| Bước | Thẻ | Trận đấu xếp hạng? | Phù hợp với trận đấu? | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 5H | Không | Không | Tiếp tục | 
| 2 | 6S | Không | Không | Tiếp tục | 
| 3 | 7C | Không | Không | Tiếp tục | 
| 4 | 8H | Không | Không | Tiếp tục | 
| 5 | 9S | Không | Không | Tiếp tục | 
| Kết thúc | - | - | - | In KHÔNG | 

Ở đây, không có thẻ nào khớp, chứng tỏ mệnh đề else được kích hoạt một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(5) → O(1) | Luôn có năm thẻ, mỗi thẻ được kiểm tra một lần. | 
| Không gian | O(1) | Chỉ có một vài biến chuỗi và danh sách bàn tay được sử dụng. | 

Với số lượng thẻ cố định, thuật toán chạy trong thời gian và bộ nhớ không đáng kể, dễ dàng phù hợp trong giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    table_card = input().strip()
    hand_cards = input().strip().split()
    table_rank = table_card[0]
    table_suit = table_card[1]
    for card in hand_cards:
        if card[0] == table_rank or card[1] == table_suit:
            return "YES"
    return "NO"

# Provided samples
assert run("AS\n2H 4C TH JH AD\n") == "YES", "sample 1"
assert run("4D\n5H 6S 7C 8H 9S\n") == "NO", "sample 2"

# Custom cases
assert run("9H\n9D 2C 3S 4D 5C\n") == "YES", "rank match only"
assert run("KH\n2H 3H 4H 5H 6H\n") == "YES", "suit match multiple"
assert run("TD\n2C 3S 4H 5C 6S\n") == "NO", "no match at all"
assert run("AS\nAD 2D 3D 4D 5D\n") == "YES", "rank match with first card"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9H\n9D 2C 3S 4D 5C | CÓ | Chỉ trận đấu xếp hạng | 
| KH\n2H 3H 4H 5H 6H | CÓ | Nhiều thẻ phù hợp với | 
| TD\n2C 3S 4H 5C 6S | KHÔNG | Không có lá bài nào phù hợp với thứ hạng hoặc chất | 
| NHƯ\nAD 2D 3D 4D 5D | CÓ | Xếp hạng trận đấu lá bài đầu tiên | 

## Vỏ cạnh 

Đối với trường hợp chỉ phù hợp với trận đấu,`table_card = 7C`Và`hand_cards = 2C 3H 4S 5D 6H`, lá bài đầu tiên có chất phù hợp. Vòng lặp xác định nó ngay lập tức, in "CÓ" và thoát. Đối với trường hợp chỉ phù hợp với thứ hạng,`table_card = 5D`Và`hand_cards = 5H 2S 3C 4H 6S`, lá bài đầu tiên khớp theo thứ hạng, đồng thời kích hoạt "CÓ". Đối với một ván bài không có kết quả phù hợp, vòng lặp sẽ hoàn thành mà không tìm thấy kết quả phù hợp và`else`mệnh đề in chính xác "KHÔNG". Điều này xác nhận thuật toán xử lý chính xác tất cả các trường hợp cạnh không rõ ràng.
