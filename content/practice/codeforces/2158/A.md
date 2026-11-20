---
title: "CF 2158A - Hệ thống treo"
description: "Chúng ta được tổ chức một trận đấu bóng đá với số lượng cầu thủ cố định và một chuỗi các hình thức kỷ luật dưới hình thức thẻ vàng, thẻ đỏ. Mỗi thẻ vàng góp phần dẫn đến khả năng bị treo giò và mỗi thẻ đỏ sẽ ngay lập tức đình chỉ một cầu thủ."
date: "2026-06-08T00:11:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2158
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1067 (Div. 2)"
rating: 800
weight: 2158
solve_time_s: 71
verified: true
draft: false
---

[CF 2158A - Đình chỉ](https://codeforces.com/problemset/problem/2158/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tổ chức một trận đấu bóng đá với số lượng cầu thủ cố định và một chuỗi các hình thức kỷ luật dưới hình thức thẻ vàng, thẻ đỏ. Mỗi thẻ vàng góp phần dẫn đến khả năng bị treo giò và mỗi thẻ đỏ sẽ ngay lập tức đình chỉ một cầu thủ. Một cầu thủ có thể bị treo giò theo hai cách khác nhau: trực tiếp nhận thẻ đỏ hoặc tích lũy hai thẻ vàng. 

Hạn chế chính là khi người chơi bị đình chỉ, bất kỳ lá bài nào khác thuộc về họ đều không liên quan vì chúng không còn trong trò chơi. Chúng tôi không được yêu cầu mô phỏng việc phân bổ thẻ thực tế theo thời gian mà thay vào đó xác định số lượng cầu thủ riêng biệt tối đa có thể bị đình chỉ thi đấu dựa trên tổng số thẻ vàng và thẻ đỏ. 

Do đó, đầu ra của mỗi trường hợp thử nghiệm là số lượng người chơi tối đa có thể bị buộc phải rời khỏi trò chơi, giả sử chúng ta phân phối các lá bài theo cách có lợi nhất. 

Các ràng buộc rất nhỏ, tối đa 100 người chơi cho mỗi bài kiểm tra và tối đa 500 trường hợp kiểm tra. Điều này ngay lập tức gợi ý rằng ngay cả một lý luận trực tiếp hoặc công thức thời gian không đổi cho mỗi trường hợp kiểm thử là đủ. Bất cứ điều gì thậm chí bậc hai trong n vẫn có thể vượt qua một cách dễ dàng, nhưng cấu trúc của bài toán gợi ý rõ ràng rằng chúng ta đang tìm kiếm một phân bổ tham lam dạng đóng hơn là một mô phỏng. 

Một trường hợp phức tạp phát sinh khi có nhiều thẻ vàng nhưng lại có ít thẻ đỏ. Ví dụ: nếu chúng ta có nhiều màu vàng nhưng ít màu đỏ, chúng ta phải quyết định xem có nên "lãng phí" màu vàng bằng cách nhóm chúng thành từng cặp hay phân phát chúng cho người chơi hay không. Một trường hợp phạt góc khác là khi chỉ riêng số màu đỏ đã vượt quá số lượng người chơi, vì mỗi màu đỏ tương ứng với nhiều nhất một lần tạm dừng. 

Một sai lầm ngây thơ là xử lý các thẻ vàng một cách độc lập, coi mỗi thẻ vàng là một án treo giò. Điều đó không thành công vì cần có hai màu vàng cho một lần đình chỉ và các màu vàng đơn còn sót lại có thể không sử dụng được nếu không còn đủ người chơi để nhận chúng theo cặp. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng việc gán từng thẻ vàng và thẻ đỏ cho từng cầu thủ, phân nhánh trên tất cả các nhiệm vụ có thể xảy ra. Mỗi thẻ có thể được trao cho bất kỳ người chơi nào vẫn đang hoạt động và mỗi nhiệm vụ có thể thay đổi nếu người chơi bị đình chỉ. Điều này nhanh chóng dẫn đến sự bùng nổ: với mỗi thẻ có tối đa 2n thẻ, có tới n lựa chọn, khiến không gian tìm kiếm theo thứ tự n^(2n), điều này hoàn toàn không khả thi ngay cả khi n = 100. 

Quan sát quan trọng là chúng tôi thực sự không quan tâm đến danh tính của người chơi, mà chỉ có thể tạo ra bao nhiêu lần đình chỉ. Mỗi thẻ đỏ có thể tạo ra tối đa một cầu thủ bị treo giò một cách độc lập và mỗi cặp thẻ vàng cũng có thể tạo ra một cầu thủ bị treo giò. Vì người chơi không thể phân biệt được về năng lực (mỗi người chỉ có thể bị tạm dừng một lần), nên vấn đề giảm xuống việc đếm xem chúng ta có thể hình thành bao nhiêu “đơn vị treo” rời rạc từ các nguồn lực sẵn có. 

Mỗi thẻ đỏ đã là một đơn vị đầy đủ rồi. Đối với thẻ vàng, cứ hai thẻ vàng sẽ tạo thành một đơn vị. Do đó, số lượng hệ thống treo dựa trên màu vàng được giới hạn bởi tầng (y / 2). Sự kết hợp duy nhất giữa màu đỏ và màu vàng là chúng tôi không thể vượt quá tổng số n người chơi bị đình chỉ. 

Vì vậy, đáp án tối thiểu là ba đại lượng: số cầu thủ n, số thẻ đỏ r và tổng số đơn vị bị treo giò có thể r + sàn(y/2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | O(n^(2n)) | O(n) | Quá chậm | 
| Đếm Tham Lam | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán câu trả lời cho từng trường hợp thử nghiệm bằng cách sử dụng chiến lược đếm trực tiếp.

1. Tính xem có bao nhiêu cầu thủ có thể bị đình chỉ chỉ vì thẻ đỏ. Mỗi thẻ đỏ sẽ treo giò một cầu thủ riêng biệt, vì vậy điều này góp phần tạo ra khả năng bị treo giò. 
2. Tính xem có bao nhiêu thẻ vàng có thể bị treo giò. Vì cần có hai màu vàng cho mỗi lần đình chỉ, nên chúng tôi lấy sàn(y / 2). Điều này cho biết số lượng đình chỉ hoàn toàn dựa trên màu vàng. 
3. Cộng hai nguồn này lại với nhau để có được tổng số “khe treo có sẵn” mà không tính đến giới hạn người chơi. 
4. Giới hạn kết quả bằng n, vì tổng cộng chỉ có n người chơi bị đình chỉ. 
5. Xuất giá trị cuối cùng. 

Bước lý luận chính là thẻ đỏ treo giò và thẻ vàng treo giò là nguồn “tiêu thụ” độc lập của cầu thủ. Mỗi người sử dụng chính xác một người chơi và không người chơi nào được tính hai lần. 

### Tại sao nó hoạt động 

Theo bất kỳ sự sắp xếp tối ưu nào, mỗi án treo giò tương ứng với chính xác một cầu thủ bị phạt thẻ đỏ hoặc ít nhất hai thẻ vàng. Vì thẻ đỏ không thể chia và thẻ vàng chỉ quan trọng theo cặp, điều tốt nhất chúng ta có thể làm là tối đa hóa số lượng nhóm rời rạc cỡ 1 (đỏ) hoặc 2 (cặp vàng) mà chúng ta có thể hình thành, chỉ tùy thuộc vào sự sẵn có của n người chơi. Không có tương tác nào có thể làm tăng tổng số huyền phù vượt quá r + sàn(y/2), và không có sự sắp xếp lại nào có thể vượt quá n. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    y, r = map(int, input().split())

    yellow_susp = y // 2
    ans = r + yellow_susp
    print(min(n, ans))
```Việc thực hiện tuân theo công thức dẫn xuất trực tiếp. Chúng tôi tính toán có bao nhiêu cặp màu vàng đầy đủ bằng cách chia số nguyên, sau đó cộng số thẻ đỏ. Cuối cùng, chúng tôi kẹp theo n để tôn trọng số lượng người chơi có sẵn. 

Điểm tinh tế duy nhất là đảm bảo sử dụng phép chia số nguyên cho các thẻ vàng, vì bất kỳ màu vàng nào còn sót lại không thể góp phần dẫn đến án treo giò nếu không ghép đôi. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện để xem công thức hoạt động như thế nào. 

### Ví dụ 1 

đầu vào: 

n = 3, y = 1, r = 2 

| Bước | r | y // 2 | r + y//2 | phút(n, kết quả) | 
| --- | --- | --- | --- | --- | 
| Tính hiệu ứng đỏ | 2 | - | 2 | - | 
| Tính cặp màu vàng | - | 0 | 2 | - | 
| Kết hợp | - | - | 2 | 2 | 
| Áp dụng giới hạn | - | - | - | 2 | 

Điều này cho thấy dù chỉ có 3 người chơi nhưng chỉ có 2 người có thể bị treo giò vì không có đủ cấu trúc màu vàng để tạo ra nhiều lần treo giò hơn. 

### Ví dụ 2 

đầu vào: 

n = 10, y = 11, r = 5 

| Bước | r | y // 2 | r + y//2 | phút(n, kết quả) | 
| --- | --- | --- | --- | --- | 
| Tính hiệu ứng đỏ | 5 | - | 5 | - | 
| Tính cặp màu vàng | - | 5 | 10 | - | 
| Kết hợp | - | - | 10 | 10 | 
| Áp dụng giới hạn | - | - | - | 10 | 

Trường hợp này cho thấy một tình huống mà số thẻ vàng đóng góp đáng kể và tổng số thẻ vàng đã bão hòa chính xác số lượng cầu thủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng các phép toán số học có thời gian không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp phù hợp một cách thoải mái trong các ràng buộc vì ngay cả số lượng trường hợp thử nghiệm tối đa cũng chỉ yêu cầu tổng cộng vài trăm thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        y, r = map(int, input().split())
        out.append(str(min(n, r + y // 2)))
    return "\n".join(out) + "\n"

# provided samples
assert run("""5
3
1 2
2
0 0
4
6 0
3
3 3
10
11 5
""") == """2
0
3
3
10
"""

# custom cases
assert run("""1
1
0 0
""") == "0\n", "no cards"

assert run("""1
5
10 0
""") == "5\n", "only yellows"

assert run("""1
5
0 10
""") == "5\n", "only reds capped by n"

assert run("""1
4
1 10
""") == "4\n", "mix capped by n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1, 0 0 | 0 | không bị đình chỉ | 
| 5, 10 0 | 5 | độ bão hòa ghép đôi màu vàng | 
| 5, 0 10 | 5 | tràn màu đỏ được giới hạn bởi n | 
| 4, 1 10 | 4 | trường hợp tràn kết hợp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi có nhiều màu vàng nhưng không thể kết hợp tất cả thành hệ thống treo do giới hạn người chơi. Ví dụ: n = 4, y = 10, r = 0. Thuật toán tính y // 2 = 5, nhưng r + y // 2 = 5 vượt quá n. Câu trả lời cuối cùng đúng là 4 vì chỉ có 4 người chơi tồn tại. Bất kỳ nỗ lực nào nhằm gán màu vàng một cách tham lam cho mỗi người chơi mà không nhận ra giới hạn chung sẽ bị tính quá mức một cách không chính xác. 

Một trường hợp khác là khi màu đỏ đã vượt quá n. Với n = 3, r = 5, y = 0, chúng tôi tính toán 5 lần treo giò của quân đỏ, nhưng chỉ tồn tại 3 người chơi, vì vậy câu trả lời là 3. Công thức xử lý chính xác điều này thông qua min(n, ...), ngăn chặn việc gán quá mức không hợp lệ. 

Trường hợp khó phát hiện cuối cùng là khi có số lượng màu vàng lẻ. Với n = 6, y = 5, r = 0, chúng ta nhận được y // 2 = 2. Một màu vàng còn lại là không thể sử dụng được và câu trả lời là 2, phù hợp với thực tế là một màu vàng phải không được ghép đôi và không thể tự tạo thành hệ thống treo.
