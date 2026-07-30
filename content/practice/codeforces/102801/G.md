---
title: "CF 102801G - Halli Galli"
description: "Halli Galli là một bài toán mô phỏng về việc nhiều người chơi lần lượt tiết lộ các thẻ trái cây. Người chơi hành động theo chu kỳ, vì vậy sau khi người chơi cuối cùng thực hiện một lượt, người chơi đầu tiên sẽ bắt đầu lại."
date: "2026-07-30T06:02:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102801
codeforces_index: "G"
codeforces_contest_name: "The 14th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102801
solve_time_s: 266
verified: true
draft: false
---

[CF 102801G - Halli Galli](https://codeforces.com/problemset/problem/102801/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Halli Galli là một bài toán mô phỏng về việc nhiều người chơi lần lượt tiết lộ các thẻ trái cây. Người chơi hành động theo chu kỳ, vì vậy sau khi người chơi cuối cùng thực hiện một lượt, người chơi đầu tiên sẽ bắt đầu lại. Mỗi người chơi chỉ giữ lại lá bài mới nhất của mình vì lá bài mới sẽ che phủ lá bài trước đó. 

Sau mỗi thẻ được tiết lộ, chúng tôi kiểm tra tất cả các thẻ hiển thị. Đối với mỗi loại trái cây trong số Táo, Chuối, Nho và Lê, chúng tôi đếm xem hiện có bao nhiêu loại trái cây thuộc loại đó. Nếu có đúng năm loại quả thì chuông sẽ được ấn một lần cho loại đó. Câu trả lời là tổng số lần nhấn chuông ở tất cả các lượt. Tuyên bố ban đầu xác định tối đa 100 trường hợp thử nghiệm, với tối đa 100 lượt cho mỗi trường hợp và tối đa 6 người chơi. 

Các giới hạn nhỏ là manh mối cho thấy đây chủ yếu là vấn đề về triển khai và mô phỏng. Ngay cả việc kiểm tra số lượng trái cây từ đầu sau mỗi lượt cũng rẻ. Chỉ có bốn loại trái cây, vì vậy việc mô phỏng trực tiếp chỉ cần một lượng công việc không đổi mỗi lượt. Một giải pháp có cấu trúc lớn không cần thiết hoặc quá trình tiền xử lý phức tạp đang giải quyết một vấn đề khó hơn vấn đề đã cho. 

Nguồn sai lầm chính là việc xử lý quy tắc thay thế một cách chính xác. Người chơi không thêm thẻ mãi mãi. Thẻ cũ của họ biến mất khi họ tiết lộ một thẻ mới. 

Ví dụ:```
1
2 1
A 5
A 5
```Đầu ra đúng là:```
2
```Sau lượt đầu tiên, các thẻ hiển thị có năm quả táo nên chuông sẽ reo một lần. Sau lượt thứ hai, thẻ cũ được thay thế bằng thẻ táo mới, không được thêm vào bên trên. Vẫn còn năm quả táo nên chuông lại reo. Việc thực hiện bất cẩn mà giữ lại mọi quân bài đã chơi sẽ đếm được mười quả táo và tạo ra kết quả không chính xác. 

Một trường hợp đặc biệt khác là khi nhiều loại quả đạt tới năm loại cùng một lúc.```
1
3 3
A 5
B 5
G 5
```Đầu ra đúng là:```
3
```Mỗi loại trái cây độc lập đóng góp một lần nhấn chuông. Việc chỉ kiểm tra xem có quả nào đạt đến năm hay không thay vì đếm tất cả các loại phù hợp sẽ làm mất các lần đẩy thêm này. 

Trường hợp ranh giới cuối cùng xuất hiện khi người chơi không có lá bài trước đó.```
1
1 6
P 1
```Đầu ra đúng là:```
0
```Lượt đầu tiên chỉ được thêm thẻ mới. Không có gì để loại bỏ khỏi vị trí của người chơi đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lưu trữ tất cả các thẻ hiện có, mô phỏng mỗi lượt và sau mỗi lượt quét thẻ của mọi người chơi để xây dựng lại tổng số bốn quả. Điều này đúng vì trạng thái trò chơi được mô tả hoàn toàn bằng các thẻ hiện có. Với nhiều nhất sáu người chơi và bốn loại trái cây, ngay cả phương pháp trực tiếp này cũng đủ nhanh ở đây. 

Ý tưởng tương tự có thể được xem xét tổng quát hơn. Trạng thái hiển thị chỉ thay đổi ở vị trí của người chơi đến lượt. Khi người chơi đó tiết lộ một lá bài, một lá bài cũ có thể biến mất và một lá bài mới xuất hiện. Việc tính toán lại mọi thứ sau mỗi lần di chuyển là không cần thiết vì hầu hết trạng thái không thay đổi. 

Quan sát quan trọng là chúng ta chỉ cần tổng số lượng mỗi loại trái cây hiện có. Khi người chơi đổi bài, chúng ta có thể trừ hoa quả ở lá bài cũ và cộng hoa quả từ lá bài mới. Tổng số hoa quả luôn thể hiện trạng thái hiện tại của bàn cờ nên sau mỗi lượt chúng ta chỉ cần bốn lần so sánh để quyết định chuông reo bao nhiêu lần. 

Lực lượng vũ phu hoạt động vì số lượng người chơi rất ít, nhưng cách tiếp cận cập nhật gia tăng phù hợp trực tiếp với cấu trúc của trò chơi. Nó giảm bớt công việc từ việc quét liên tục tất cả người chơi đến việc cập nhật một thẻ và kiểm tra bốn quầy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NK) | O(K) | Được chấp nhận với giới hạn nhất định | 
| Tối ưu | O(N) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ thẻ hiện tại của mọi người chơi. Ban đầu, mọi người chơi đều không có thẻ. 
2. Giữ bốn quầy biểu thị số lượng quả có thể nhìn thấy được của mỗi loại. Những bộ đếm này là thông tin đầy đủ cần thiết để quyết định xem chuông có đổ chuông hay không. 
3. Đối với mỗi lượt, hãy xác định người chơi nào đang hành động bằng cách sử dụng chỉ số lượt theo số lượng người chơi. 
4. Nếu người chơi đó đã có thẻ, hãy loại bỏ số lượng trái cây của người đó khỏi quầy trái cây tương ứng. Thẻ cũ không còn hiển thị sau khi người chơi tiết lộ thẻ mới. 
5. Đọc thẻ mới, lưu nó làm thẻ hiện tại của người chơi và thêm số lượng trái cây của nó vào quầy trái cây phù hợp. 
6. Kiểm tra bốn quầy hoa quả. Mỗi bộ đếm bằng năm sẽ thêm một vào câu trả lời vì mỗi loại trái cây được tính độc lập. 
7. Sau khi xử lý tất cả các lượt, xuất ra số lần đẩy chuông tích lũy. 

Tại sao nó hoạt động: 

Điều bất biến là sau mỗi lượt được xử lý, bốn quầy trái cây khớp chính xác với các loại trái cây hiển thị trên thẻ hiện tại của tất cả người chơi. Ban đầu cả hai đều trống rỗng. Khi người chơi thay đổi thẻ, việc loại bỏ thẻ cũ và thêm thẻ mới sẽ cập nhật bộ đếm về trạng thái hiển thị mới. Vì điều kiện chuông chỉ phụ thuộc vào bốn tổng này nên việc kiểm tra chúng sau mỗi lần cập nhật sẽ đưa ra chính xác số lần đẩy theo yêu cầu của quy tắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    index = {'A': 0, 'B': 1, 'G': 2, 'P': 3}

    for _ in range(t):
        n, k = map(int, input().split())

        cards = [None] * k
        counts = [0] * 4
        total = 0

        for turn in range(n):
            ch, x = input().split()
            x = int(x)

            player = turn % k

            if cards[player] is not None:
                old_ch, old_x = cards[player]
                counts[index[old_ch]] -= old_x

            cards[player] = (ch, x)
            counts[index[ch]] += x

            for value in counts:
                if value == 5:
                    total += 1

        ans.append(str(total))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`cards`mảng lưu trữ thẻ mới nhất cho mỗi người chơi. Chỉ mục của nó là số người chơi, vì vậy việc tìm thẻ để loại bỏ trước khi cập nhật mất nhiều thời gian. 

các`counts`mảng chứa số lượng hiển thị hiện tại của mỗi loại trái cây. Việc ánh xạ từ ký tự trái cây đến chỉ mục mảng sẽ tránh việc kiểm tra điều kiện lặp đi lặp lại và giữ cho mỗi lần cập nhật luôn được cập nhật liên tục. 

Thứ tự của các hoạt động quan trọng. Thẻ trước đó phải được xóa trước khi thêm thẻ mới vì thẻ mới sẽ thay thế thẻ cũ. Việc kiểm tra tình trạng chuông chỉ xảy ra sau cả hai thao tác, vì trò chơi sẽ đánh giá trạng thái hiển thị hoàn chỉnh sau khi lượt chơi kết thúc. 

Không có nguy cơ tràn số nguyên trong Python vì số vòng quay tối đa là nhỏ. Câu trả lời là nhiều nhất là bốn lần nhấn chuông mỗi lượt, vì vậy nó vẫn rất nhỏ. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
1
5 3
A 5
B 2
B 3
G 1
P 5
```Mô phỏng là: 

| Xoay | Người chơi | Thẻ Mới | Số lượng trái cây A,B,G,P | Chuông Đẩy Lượt Này | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | A 5 | 5,0,0,0 | 1 | 1 | 
| 2 | 1 | B 2 | 5,2,0,0 | 1 | 2 | 
| 3 | 2 | B 3 | 5,5,0,0 | 2 | 4 | 
| 4 | 0 | G 1 | 0,5,1,0 | 1 | 5 | 
| 5 | 1 | P 5 | 0,3,1,5 | 1 | 6 | 

Lượt thứ tư chứng minh tại sao việc thay thế là cần thiết. Năm quả táo của Người chơi 0 biến mất khi thẻ nho được tiết lộ. 

Một ví dụ thứ hai:```
1
4 2
A 3
A 2
B 5
A 5
```| Xoay | Người chơi | Thẻ Mới | Số lượng trái cây A,B,G,P | Chuông Đẩy Lượt Này | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | A 3 | 3,0,0,0 | 0 | 0 | 
| 2 | 1 | A 2 | 5,0,0,0 | 1 | 1 | 
| 3 | 0 | B 5 | 2,5,0,0 | 1 | 2 | 
| 4 | 1 | A 5 | 7,0,0,0 | 0 | 2 | 

Dấu vết này xác nhận rằng bộ đếm chỉ mô tả các thẻ hiển thị. Thẻ táo đầu tiên được loại bỏ ở lượt thứ ba và thẻ táo của người chơi thứ hai được thay thế vào lượt thứ tư. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi lượt thực hiện một số lần cập nhật liên tục và kiểm tra bốn quầy hoa quả. | 
| Không gian | O(K) | Chúng tôi lưu trữ một thẻ hiện tại cho mỗi người chơi. | 

Đầu vào lớn nhất chứa 100 ca kiểm thử, mỗi ca có 100 lượt, do đó tổng số lượt mô phỏng là nhỏ. Chiến lược cập nhật liên tục dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

# provided sample
assert run("""1
5 3
A 5
B 2
B 3
G 1
P 5
""") == "6\n", "sample 1"

# minimum size
assert run("""1
1 1
A 5
""") == "1\n", "single card"

# no bell
assert run("""1
3 2
A 1
B 1
G 1
""") == "0\n", "no fruit reaches five"

# replacement case
assert run("""1
2 1
A 5
A 5
""") == "2\n", "replacement instead of accumulation"

# multiple fruits at once
assert run("""1
3 3
A 5
B 5
G 5
""") == "3\n", "multiple simultaneous pushes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một người chơi với một lá bài | 1 | Xử lý đầu vào kích thước tối thiểu | 
| Thẻ nhỏ có tổng số dưới năm | 0 | Tránh dương tính giả | 
| Cùng một người chơi tiết lộ hai lá bài năm quả táo | 2 | Logic thay thế đúng | 
| Ba loại quả đạt năm | 3 | Đếm từng loại trái cây hợp lệ | 

## Vỏ cạnh 

Trường hợp thay thế được xử lý vì thuật toán lưu trữ chính xác một lá bài cho mỗi người chơi. Vì:```
1
2 1
A 5
A 5
```lượt đầu tiên thêm năm quả táo và câu trả lời trở thành một. Lượt thứ hai loại bỏ năm quả táo cũ, thêm năm quả táo mới và thêm một lần nhấn chuông nữa. Câu trả lời cuối cùng là hai. 

Trường hợp nhiều quả được xử lý bằng cách kiểm tra cả bốn quầy thay vì dừng lại sau khi tìm thấy một quả khớp. Vì:```
1
3 3
A 5
B 5
G 5
```các quầy trở thành năm quầy dành cho táo, chuối và nho. Mỗi quầy đóng góp riêng biệt, đưa ra câu trả lời là ba. 

Trường hợp lá bài đầu tiên được xử lý bằng cách kiểm tra xem người chơi đã sở hữu lá bài nào chưa trước khi loại bỏ bất kỳ thứ gì. Vì:```
1
1 6
P 1
```người chơi không có thẻ trước đó nên thuật toán chỉ thêm một quả lê và báo cáo chính xác không có lần đẩy chuông nào.
