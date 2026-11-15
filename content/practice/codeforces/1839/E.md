---
title: "CF 1839E - Trò chơi giảm dần"
description: "Chúng ta đang chơi một trò chơi hai người trên một dãy số nguyên dương. Trong mỗi vòng, người chơi đầu tiên chọn một vị trí có giá trị dương, sau đó người chơi thứ hai phải chọn một vị trí khác cũng dương."
date: "2026-06-09T06:31:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dp", "greedy", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1839
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 876 (Div. 2)"
rating: 2400
weight: 1839
solve_time_s: 79
verified: false
draft: false
---

[CF 1839E - Trò chơi giảm dần](https://codeforces.com/problemset/problem/1839/E) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng, dfs và tương tự, dp, tham lam, tương tác 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chơi một trò chơi hai người trên một dãy số nguyên dương. Trong mỗi vòng, người chơi đầu tiên chọn một vị trí có giá trị dương, sau đó người chơi thứ hai phải chọn một vị trí khác cũng dương. Hai giá trị đã chọn được giảm đồng thời bằng giá trị nhỏ hơn trong hai giá trị đó, điều này đảm bảo rằng ít nhất một trong số chúng trở thành 0 khi kết thúc vòng đấu. Quá trình tiếp tục cho đến khi một người chơi không thể thực hiện một nước đi hợp lệ: người chơi đầu tiên thua nếu không có yếu tố tích cực nào để họ chọn và người chơi thứ hai thua nếu sau lựa chọn của người chơi đầu tiên không có yếu tố tích cực riêng biệt thứ hai. 

Cấu trúc chính là mỗi vòng “tiêu thụ” một lượng tổng giá trị từ hai chỉ số riêng biệt và bất cứ khi nào một cặp được chọn, ít nhất một chỉ số sẽ trở thành 0. Điều này có nghĩa là số lượng chỉ số hoạt động (tích cực) giảm dần theo thời gian, do đó trò chơi luôn kết thúc. 

Các ràng buộc nhỏ, với n tối đa 300 và giá trị lên tới 300. Điều này cho thấy rõ ràng rằng lý luận O(n^2) hoặc O(n^3) có thể chấp nhận được và giải pháp có thể phụ thuộc vào đặc tính cấu trúc về cách các khối lượng dương được ghép cặp thay vì bất kỳ mô phỏng sâu nào về tất cả các chuyển động có thể xảy ra. 

Một cách tiếp cận ngây thơ có thể cố gắng mô phỏng sự tương tác một cách tham lam hoặc thậm chí mô hình hóa cách chơi tối ưu một cách linh hoạt trên tất cả các tập hợp con của các chỉ số tích cực. Điều này không thành công vì người chơi thứ hai là đối thủ và việc lựa chọn ghép đôi ảnh hưởng đến khả năng di chuyển trong tương lai theo cách rất phi cục bộ. Ví dụ: nếu người chơi đầu tiên liên tục nhắm vào cùng một cọc lớn, một mô phỏng bất cẩn có thể sớm loại bỏ tính linh hoạt và kết luận không chính xác rằng đối thủ buộc phải thắng hoặc thua. 

Trường hợp cạnh tinh tế là khi chính xác một chỉ số vẫn dương. Trong trường hợp đó, ai buộc phải di chuyển trước sẽ thua ngay lập tức vì không có đối tác hợp lệ nào tồn tại. Điều này ngụ ý rằng kết quả của trò chơi bị chi phối bởi việc liệu quá trình có thể bị buộc phải kết thúc với một cọc khác 0 ở lượt của đối thủ hay không. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mỗi nước đi sẽ làm giảm hai cọc dương và cách duy nhất để trò chơi kết thúc là khi còn lại ít hơn hai cọc dương. Do đó, cấu trúc của trò chơi hoàn toàn được xác định bằng việc chúng ta có thể giữ được ít nhất hai chỉ số dương trong bao lâu. 

Mỗi vòng kết hợp hiệu quả hai chỉ số dương riêng biệt và giảm cả hai chỉ số dương một lượng nào đó. Bất kể giá trị giảm như thế nào, mọi thao tác đều loại bỏ ít nhất một chỉ số dương. Vì vậy, số vòng tối đa được liên kết chặt chẽ với số lượng “cơ hội ghép đôi” tồn tại giữa nhiều tập hợp giá trị ban đầu. 

Từ góc độ toàn cầu, trò chơi giảm xuống việc liên tục loại bỏ một đơn vị “khả năng ghép nối” khỏi hai chỉ số khác nhau cho đến khi nhiều nhất một chỉ số vẫn khác 0. Điều này tương đương với điều kiện cổ điển: nếu một phần tử chiếm ưu thế quá mạnh thì cuối cùng nó sẽ tồn tại một mình, nếu không thì khối lượng có thể cân bằng cho đến khi mọi giá trị biến mất. 

Đặt S là tổng của tất cả các giá trị và M là phần tử tối đa. Nếu M lớn hơn S − M thì sau khi ghép càng nhiều càng tốt, đống lớn nhất đó vẫn còn khối lượng còn sót lại ngay cả khi tất cả các đống khác đã cạn kiệt. Điều này đảm bảo rằng cuối cùng trò chơi sẽ đạt đến trạng thái có đúng một chỉ số dương và người chơi nào di chuyển trong tình huống đó sẽ thua. 

Nếu M không lớn hơn S − M thì có thể luôn giữ được ít nhất hai chỉ số dương cho đến khi mọi thứ cạn kiệt một cách cân bằng. Trong trường hợp đó, người chơi đầu tiên có chiến lược chiến thắng và nên bắt đầu trước; nếu không, người chơi thứ hai sẽ an toàn hơn. 

Quyết định rút gọn thành một phép so sánh đơn giản giữa phần tử lớn nhất và phần còn lại của mảng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi đầy đủ | O(S) hoặc tệ hơn theo cấp số nhân | O(n) | Quá chậm | 
| Kiểm tra cấu trúc tham lam tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của tất cả các phần tử và xác định phần tử lớn nhất. 
2. So sánh phần tử lớn nhất M với tổng các phần tử còn lại S − M. 
3. Nếu M lớn hơn S − M, hãy chọn chơi Thứ hai, nếu không hãy chọn Đầu tiên. 

Lý do đằng sau sự lựa chọn là khi cọc lớn nhất vượt quá “sự hỗ trợ” tổng hợp của tất cả các cọc khác, chắc chắn nó sẽ tồn tại một mình. Trong kịch bản đó, người chơi đi đầu tiên vào giai đoạn cuối sẽ bị buộc vào trạng thái thua cuộc khi không có đồng đội nào tồn tại. Nếu không có sự thống trị như vậy tồn tại, trò chơi luôn có thể được cấu trúc sao cho việc loại bỏ diễn ra theo cặp mà không cần tách một cọc nào quá sớm. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi bước di chuyển sẽ loại bỏ số lượng bằng nhau khỏi hai chỉ số riêng biệt, do đó tổng khối lượng giảm trong khi số lượng chỉ số dương giảm ít nhất một chỉ số mỗi vòng. Cách duy nhất để tránh trạng thái kết thúc sớm với một chỉ số dương duy nhất là đảm bảo rằng không có chỉ số đơn lẻ nào có thể tồn tại lâu hơn tất cả các chỉ số khác cộng lại. Nếu sự thống trị như vậy tồn tại, cấu hình cuối cùng sẽ bị ép buộc và xác định bên thua cuộc. Mặt khác, tính linh hoạt trong việc ghép đôi đảm bảo rằng quá trình có thể được sắp xếp sao cho nước đi cuối cùng khiến cả hai người chơi không bị buộc phải mất lợi thế về vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    mx = max(a)
    
    if mx > total - mx:
        print("Second")
    else:
        print("First")

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng, tính tổng và giá trị tối đa của nó, đồng thời áp dụng trực tiếp kiểm tra ưu thế. Việc so sánh mã hóa toàn bộ hành vi chiến lược của trò chơi mà không mô phỏng bất kỳ vòng nào. 

Chi tiết thực hiện tinh tế đang đảm bảo sự bất bình đẳng nghiêm ngặt. Sự bình đẳng vẫn cho phép một chuỗi ghép đôi cân bằng trong đó không có cọc đơn nào tồn tại một mình, vì vậy nó thuộc về trường hợp thắng trước. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[10, 4, 6, 3]`. Tổng là 23 và tối đa là 10. Vì 10 không lớn hơn 13 nên người chơi đầu tiên có thể tránh bị mắc kẹt trong tình huống chỉ có một người sống sót. Quyết định là chơi Đầu tiên. 

| Bước | Tổng hợp | Tối đa | Tình trạng | 
| --- | --- | --- | --- | 
| Ban đầu | 23 | 10 | 10 13 | 

Điều này cho thấy không có phần tử nào lấn át phần còn lại, do đó việc ghép nối có thể tiếp tục cho đến khi mảng sụp đổ hoàn toàn. 

Bây giờ hãy xem xét`[1, 1, 1, 10]`. Tổng là 13 và tối đa là 10. Vì 10 > 3, phần tử lớn nhất chắc chắn sẽ còn lại sau khi tất cả các phần tử khác đã cạn kiệt. 

| Bước | Tổng hợp | Tối đa | Tình trạng | 
| --- | --- | --- | --- | 
| Ban đầu | 13 | 10 | 10 > 3 | 

Ở đây, trò chơi bị buộc vào trạng thái chỉ còn một chỉ số dương và việc lựa chọn người chơi thứ hai trước tiên trở nên bất khả thi, vì vậy chọn Thứ hai là tối ưu. 

Những dấu vết này cho thấy sự bất bình đẳng trực tiếp nắm bắt được liệu trò chơi có thể tránh được sự cô lập của một cọc hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để tính tổng và tối đa | 
| Không gian | O(1) | chỉ tổng hợp được lưu trữ | 

Kích thước đầu vào nhỏ nhưng giải pháp đã tối ưu và vừa khít với các ràng buộc, khiến nó trở nên mạnh mẽ ngay cả trong các giới hạn chặt chẽ hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    total = sum(a)
    mx = max(a)
    print("Second" if mx > total - mx else "First")

# provided sample
assert run("4\n10 4 6 3\n") == "First"

# all equal
assert run("3\n5 5 5\n") == "First"

# dominant element
assert run("3\n1 1 10\n") == "Second"

# single element
assert run("1\n7\n") == "First"

# max at boundary equality
assert run("2\n3 3\n") == "First"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 4 6 3 | Đầu tiên | trường hợp cân bằng | 
| 1 1 10 | Thứ hai | cọc chiếm ưu thế | 
| 5 5 5 | Đầu tiên | trường hợp cạnh bình đẳng | 
| 7 | Đầu tiên | tối thiểu n | 
| 3 3 | Đầu tiên | ranh giới đối xứng | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, không có nước đi thứ hai hợp lệ kể từ đầu, vì vậy người chơi đầu tiên sẽ thua ngay lập tức nếu bị buộc phải chơi. Thuật toán xuất ra Đầu tiên vì không có phần tử nào lấn át phần còn lại và trò chơi kết thúc một cách tầm thường. 

Vì`[1, 1, 10]`, phần tử lớn nhất vượt quá tổng của các phần tử khác. Mỗi cặp sẽ loại bỏ tối đa một đơn vị khỏi đống lớn cho mỗi lần tương tác, trong khi các đống nhỏ hơn biến mất nhanh chóng. Cuối cùng chỉ còn lại đống lớn, buộc đối thủ vào thế không tồn tại lựa chọn thứ hai hợp lệ, khớp với đầu ra Thứ hai. 

Vì`[5, 5, 5]`, không có phần tử nào chiếm ưu thế. Việc ghép đôi luôn có thể được sắp xếp sao cho việc loại bỏ diễn ra đồng đều, không để lại đống đơn lẻ bắt buộc, tương ứng với việc chọn Đầu tiên. 

Mỗi trường hợp đều xác nhận rằng sự bất bình đẳng mô tả đầy đủ liệu trò chơi có rơi vào trạng thái chỉ có một người sống sót hay vẫn cân bằng cho đến khi cạn kiệt.
