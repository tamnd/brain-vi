---
title: "CF 102775E - \u041a\u0430\u043c\u0435\u043d\u044c, \u043d\u043e\u0436\u043d\u0438\u0446\u044b, \u0431\u0443\u043c\u0430\u0433\u0430..."
description: "Trò chơi có bảy nước đi có thể thực hiện được và hai người chơi mỗi người chọn một nước đi. Từ đầu tiên trong đầu vào là chuyển động của người điều khiển vui vẻ và từ thứ hai là chuyển động của người điều khiển buồn."
date: "2026-07-27T20:38:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "E"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 42
verified: true
draft: false
---

[CF 102775E - \u041a\u0430\u043c\u0435\u043d\u044c, \u043d\u043e\u0436\u043d\u0438\u0446\u044b, \u0431\u0443\u043c\u0430\u0433\u0430...](https://codeforces.com/problemset/problem/102775/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có bảy nước đi có thể thực hiện được và hai người chơi mỗi người chọn một nước đi. Từ đầu tiên trong đầu vào là chuyển động của người điều khiển vui vẻ và từ thứ hai là chuyển động của người điều khiển buồn. Nhiệm vụ là xác định kết quả từ góc độ của bộ điều khiển vui vẻ: đầu ra`1`nếu người chơi đầu tiên thắng,`-1`nếu người chơi thứ hai thắng, và`0`nếu cả hai cùng chọn một nước đi. 

Số lần di chuyển có thể được cố định là bảy, do đó không có kích thước đầu vào lớn để tối ưu hóa. Khó khăn chính không phải là hiệu suất mà là việc thể hiện các quy tắc một cách chính xác. Mô phỏng trực tiếp tất cả các trường hợp là đủ vì chỉ có một số lượng nhỏ các cặp có thể xảy ra. Ngay cả một phương pháp kiểm tra mọi sự kết hợp có thể cũng chỉ thực hiện được một khối lượng công việc không đổi. 

Đầu vào chứa chính xác hai chuỗi, do đó mức sử dụng bộ nhớ đương nhiên là không đổi. Không cần cấu trúc dữ liệu phát triển theo kích thước đầu vào. Giới hạn thời gian có thể dễ dàng được đáp ứng bằng bất kỳ giải pháp nào chỉ thực hiện một số tra cứu hoặc so sánh từ điển. 

Những sai lầm chính đến từ việc xử lý không đầy đủ các quy tắc. Việc thực hiện bất cẩn có thể chỉ lưu giữ một nửa số quan hệ thắng cuộc và quên rằng người chiến thắng phụ thuộc vào thứ tự của hai người chơi. Ví dụ: với đầu vào:```
stone scissors
```đầu ra đúng là:```
1
```vì đá thắng kéo. Một chương trình chỉ kiểm tra xem nước đi thứ hai có thể đánh bại nước đi đầu tiên hay không có thể đảo ngược kết quả. 

Một sai lầm phổ biến khác là quên trường hợp rút thăm. Ví dụ:```
ax ax
```có đầu ra đúng:```
0
```bởi vì các nước đi bằng nhau luôn dẫn đến kết quả hòa. Một chương trình chỉ tìm kiếm các cặp thắng có thể báo cáo thua không chính xác vì nó không thể tìm thấy mối quan hệ thắng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là liệt kê mọi cặp chiến thắng cho người điều khiển vui vẻ. Khi nước đi đầu tiên và nước đi thứ hai được đọc, chúng tôi kiểm tra xem cặp chính xác này có xuất hiện trong các kết hợp chiến thắng hay không. Nếu có thì câu trả lời là`1`. Nếu cặp đảo ngược xuất hiện, người điều khiển thứ hai sẽ thắng và câu trả lời là`-1`. Nếu cả hai đều không xảy ra thì các nước đi đều bằng nhau và câu trả lời là`0`. 

Ý tưởng bạo lực này đã đủ nhanh vì trò chơi chỉ có bảy nước đi. Có tối đa 49 cặp nước đi có thứ tự khả thi và việc kiểm tra chúng cần một số thao tác cố định. 

Việc triển khai có cấu trúc chặt chẽ hơn sẽ lưu trữ tất cả các mối quan hệ chiến thắng trong một tập hợp. Điều này thay đổi giải pháp từ việc kiểm tra nhiều điều kiện theo cách thủ công sang thực hiện một bài kiểm tra thành viên duy nhất. Nhận xét quan trọng là các quy tắc mô tả một mối quan hệ có hướng: một nước đi có thể đánh bại một nước đi khác, nhưng điều ngược lại không phải lúc nào cũng đúng. Một tập hợp các cặp có thứ tự đại diện chính xác cho cấu trúc này. 

Giải pháp brute-force hoạt động vì không gian trạng thái rất nhỏ nhưng có thể khó duy trì nếu số lần di chuyển tăng lên. Nhận xét rằng các quy tắc chỉ đơn giản là một tập hợp các cạnh có hướng cho phép chúng ta biểu diễn trò chơi một cách gọn gàng và trả lời truy vấn bằng cách tra cứu theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai nước đi đã chọn. Giá trị đầu tiên thuộc về bộ điều khiển vui vẻ và giá trị thứ hai thuộc về bộ điều khiển buồn, vì vậy việc giữ nguyên thứ tự này là cần thiết để xác định dấu của câu trả lời. 
2. Lưu trữ tất cả các cặp nước đi thắng trong đó phần tử đầu tiên đánh bại phần tử thứ hai. Mỗi cặp được định hướng bởi vì`stone`đánh đập`scissors`không có nghĩa là`scissors`nhịp đập`stone`. 
3. Kiểm tra xem cặp`(first_move, second_move)`có mặt trong tập chiến thắng. Nếu nó hiện diện, người điều khiển vui vẻ sẽ thắng và câu trả lời là`1`. 
4. Kiểm tra xem cặp`(second_move, first_move)`đang có mặt. Nếu nó hiện diện, người điều khiển buồn sẽ thắng vì nước đi của người chơi thứ hai đánh bại nước đi của người chơi đầu tiên. 
5. Nếu không có cặp thứ tự nào tồn tại thì khả năng duy nhất còn lại là cả hai nước đi đều giống nhau. đầu ra`0`. 

Lý do điều này có hiệu quả là vì mọi trạng thái trò chơi không hòa đều có chính xác một hướng thắng. Các mối quan hệ được lưu trữ chứa mọi chiến thắng có thể xảy ra, vì vậy một trong hai cặp có thứ tự phải được tìm thấy khi có người chiến thắng. Các bước đi ngang nhau không tạo ra mối quan hệ chiến thắng theo cả hai hướng, điều này đương nhiên tạo ra một trận hòa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    first, second = input().split()

    wins = {
        ("stone", "scissors"),
        ("stone", "controller"),
        ("stone", "knife"),
        ("scissors", "paper"),
        ("scissors", "pliers"),
        ("scissors", "controller"),
        ("paper", "ax"),
        ("paper", "stone"),
        ("paper", "pliers"),
        ("pliers", "stone"),
        ("pliers", "knife"),
        ("pliers", "ax"),
        ("knife", "controller"),
        ("knife", "paper"),
        ("knife", "scissors"),
        ("ax", "knife"),
        ("ax", "scissors"),
        ("ax", "stone"),
        ("controller", "pliers"),
        ("controller", "ax"),
        ("controller", "paper"),
    }

    if (first, second) in wins:
        print(1)
    elif (second, first) in wins:
        print(-1)
    else:
        print(0)

if __name__ == "__main__":
    solve()
```Phần đầu vào đọc chính xác hai chuỗi và giữ nguyên thứ tự của chúng vì giá trị đầu tiên luôn đại diện cho bộ điều khiển vui vẻ. 

các`wins`tập hợp chứa các cặp có thứ tự. Việc sử dụng một bộ sẽ tránh được một chuỗi dài các câu lệnh có điều kiện và làm cho mối quan hệ giữa các quy tắc trò chơi và việc triển khai trở nên trực tiếp. Mỗi lần tra cứu là thời gian không đổi. 

Lần kiểm tra tư cách thành viên đầu tiên hỏi liệu người điều khiển vui vẻ có nước đi thắng hay không. Lần kiểm tra thứ hai đảo ngược cặp và hỏi liệu bộ điều khiển buồn có thắng hay không. Các nước đi bằng nhau hoặc bất kỳ cặp nước đi nào không thể di chuyển đến nhánh cuối cùng và tạo ra kết quả hòa. 

Không có phép tính ranh giới, vòng lặp hoặc phép toán số nên các vấn đề như lỗi tràn hoặc lỗi sai một không thể xuất hiện. Chi tiết triển khai duy nhất cần được quan tâm là viết mọi mối quan hệ chiến thắng theo đúng hướng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
stone paper
```Dấu vết: 

| Bước | Bước đi đầu tiên | Bước thứ hai | Cặp đã kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | đá | giấy | (đá, giấy) | Không thắng | 
| 2 | đá | giấy | (giấy, đá) | Trong chiến thắng | 
| 3 | đá | giấy | Đầu ra | -1 | 

Người chơi đầu tiên không có mối quan hệ chiến thắng với giấy. Mối quan hệ ngược tồn tại vì giấy đánh bại được đá nên kẻ điều khiển buồn bã sẽ thắng. 

### Mẫu 2 

đầu vào:```
ax ax
```Dấu vết: 

| Bước | Bước đi đầu tiên | Bước thứ hai | Cặp đã kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | rìu | rìu | (rìu, rìu) | Không thắng | 
| 2 | rìu | rìu | (rìu, rìu) | Không thắng | 
| 3 | rìu | rìu | Đầu ra | 0 | 

Không người chơi nào có quan hệ chiến thắng vì cả hai đều chọn cùng một nước đi. Thuật toán xác định chính xác trận hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện hai tra cứu tập hợp có chứa một số bước di chuyển cố định. | 
| Không gian | O(1) | Mối quan hệ chiến thắng chứa một số cặp cố định không phụ thuộc vào kích thước đầu vào. | 

Giải pháp dễ dàng phù hợp với các giới hạn vì toàn bộ tính toán là thời gian không đổi và chỉ sử dụng một lượng bộ nhớ cố định nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    first, second = input().split()

    wins = {
        ("stone", "scissors"),
        ("stone", "controller"),
        ("stone", "knife"),
        ("scissors", "paper"),
        ("scissors", "pliers"),
        ("scissors", "controller"),
        ("paper", "ax"),
        ("paper", "stone"),
        ("paper", "pliers"),
        ("pliers", "stone"),
        ("pliers", "knife"),
        ("pliers", "ax"),
        ("knife", "controller"),
        ("knife", "paper"),
        ("knife", "scissors"),
        ("ax", "knife"),
        ("ax", "scissors"),
        ("ax", "stone"),
        ("controller", "pliers"),
        ("controller", "ax"),
        ("controller", "paper"),
    }

    if (first, second) in wins:
        return "1"
    if (second, first) in wins:
        return "-1"
    return "0"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve()
    finally:
        sys.stdin = old_stdin

assert run("stone paper\n") == "-1", "sample 1"
assert run("ax ax\n") == "0", "sample 2"

assert run("stone scissors\n") == "1", "direct win"
assert run("controller paper\n") == "1", "controller special move"
assert run("paper ax\n") == "1", "reverse direction check"
assert run("knife stone\n") == "-1", "second player win"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giấy đá | -1 | Kiểm tra trường hợp người chơi thứ hai thắng. | 
| rìu rìu | 0 | Kiểm tra điều kiện rút bài. | 
| kéo đá | 1 | Kiểm tra chiến thắng trực tiếp của người chơi đầu tiên. | 
| giấy điều khiển | 1 | Kiểm tra một trong những tương tác di chuyển đặc biệt. | 
| rìu giấy | 1 | Kiểm tra xem hướng lưu trữ có được xử lý chính xác hay không. | 
| đá dao | -1 | Kiểm tra đảo ngược quan hệ chiến thắng. | 

## Vỏ cạnh 

Đối với các nước đi bằng nhau, thuật toán sẽ kiểm tra cả hai hướng và không tìm thấy cặp chiến thắng nào. Đối với đầu vào:```
ax ax
```cặp đôi`(ax, ax)`vắng mặt trong set thắng và cặp đảo ngược giống hệt và cũng vắng mặt. Đầu ra của nhánh cuối cùng`0`, phù hợp với quy luật. 

Để giành chiến thắng ngược, thứ tự của yếu tố đầu vào rất quan trọng. Với:```
knife stone
```cặp đôi`(knife, stone)`không phải là một mối quan hệ chiến thắng. Cặp đảo ngược`(stone, knife)`tồn tại vì đá thắng được dao. Đầu ra của thuật toán`-1`, gán chính xác chiến thắng cho người điều khiển thứ hai. 

Đối với hành động đặc biệt`controller`, việc triển khai xử lý nó giống như mọi động thái khác. Với:```
controller ax
```cặp đôi`(controller, ax)`tồn tại, do đó người điều khiển đầu tiên sẽ thắng và câu trả lời là`1`. Điều này khẳng định rằng những động thái bất thường không bị vô tình bỏ qua hoặc xử lý khác đi. 

Tôi cũng có thể điều chỉnh nội dung này thành định dạng biên tập ngắn hơn theo phong cách Codeforces nếu bạn cần nội dung nào đó gần giống với lời giải thích chính thức hơn về cuộc thi.
