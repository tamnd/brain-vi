---
title: "CF 105198J - Trò chơi Monke, Potato và Hiệp sĩ của họ"
description: "Bàn cờ là vô hạn, vì vậy thông tin duy nhất quan trọng là vị trí tương đối giữa ô bắt đầu và ô đích đã chọn. Đối với mọi trường hợp thử nghiệm, chúng tôi được cung cấp hai tọa độ cho vị trí ban đầu của hiệp sĩ và hai tọa độ cho vị trí mục tiêu."
date: "2026-06-27T03:00:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105198
codeforces_index: "J"
codeforces_contest_name: "ShellBeeHaken Presents Intra SUST Programming Contest 2024 - Replay"
rating: 0
weight: 105198
solve_time_s: 65
verified: false
draft: false
---

[CF 105198J - Trò chơi Monke, Potato và Hiệp sĩ của họ](https://codeforces.com/problemset/problem/105198/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bàn cờ là vô hạn, vì vậy thông tin duy nhất quan trọng là vị trí tương đối giữa ô bắt đầu và ô đích đã chọn. Đối với mọi trường hợp thử nghiệm, chúng tôi được cung cấp hai tọa độ cho vị trí ban đầu của hiệp sĩ và hai tọa độ cho vị trí mục tiêu. Chúng ta cần quyết định xem có cách nào để hiệp sĩ tiếp cận mục tiêu bằng cách sử dụng số lượng nước đi lẻ của hiệp sĩ hay không. Nếu con đường như vậy tồn tại thì người chiến thắng là Monke. Ngược lại, Khoai tây sẽ thắng. 

Tọa độ có thể lớn bằng$10^6$và số lượng ca kiểm thử có thể đạt tới$10^5$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào khám phá bảng, bởi vì ngay cả một tìm kiếm theo chiều rộng nhỏ xung quanh một vị trí cũng sẽ ghé thăm nhiều ô vuông và lặp lại quá trình đó$10^5$lần sẽ vượt quá các hoạt động có sẵn. Chúng ta cần quan sát thời gian liên tục hoạt động trực tiếp từ tọa độ. 

Phần khó khăn là bàn cờ là vô hạn, do đó, một cách tiếp cận ngây thơ có thể cố gắng suy luận về con đường hiệp sĩ ngắn nhất hoặc xây dựng biểu đồ tìm kiếm. Câu trả lời không phụ thuộc vào khoảng cách chính xác. Nó chỉ phụ thuộc vào đặc tính tô màu của đồ thị hiệp sĩ. 

Một lỗi phổ biến là chỉ kiểm tra xem mục tiêu có thể tiếp cận được trong một lần di chuyển hay không. Ví dụ, đầu vào`1 1 4 5`là trường hợp mục tiêu không phải là một hiệp sĩ di chuyển đi mà câu trả lời vẫn là Monke vì hiệp sĩ có thể tiếp cận nó với một số nước đi lẻ. Một sai lầm khác là cho rằng việc đến một hình vuông với số lần đi chẵn có nghĩa là cũng có thể tồn tại một tuyến đường lẻ. Vì`1 1 1 3`, màu tọa độ giống nhau nên mọi tuyến đường có thể có độ dài chẵn. Đầu ra đúng là`Potato`. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp sẽ là thực hiện tìm kiếm biểu đồ từ ô vuông bắt đầu. Mỗi ô vuông là một nút và mỗi nước đi của hiệp sĩ sẽ tạo ra một cạnh. Tìm kiếm theo chiều rộng có thể tìm thấy khoảng cách ngắn nhất tới đích và nếu khoảng cách đó là số lẻ thì Monke sẽ thắng. Vì đường đi ngắn nhất trong biểu đồ không có trọng số được BFS tìm thấy nên cách tiếp cận này là chính xác. 

Vấn đề là kích thước của biểu đồ. Mặc dù chúng ta chỉ cần một đích đến, BFS trên bảng vô hạn có thể phải kiểm tra một số lượng lớn các ô vuông. Với$10^5$trường hợp thử nghiệm, bất kỳ phương pháp dựa trên tìm kiếm nào cũng không thể thực hiện được. Trong trường hợp xấu nhất, nếu tìm kiếm truy cập$K$bình phương cho mỗi trường hợp thử nghiệm, tổng công việc sẽ trở thành$O(10^5K)$, vượt xa giới hạn. 

Nhận xét quan trọng nhất đến từ việc nhìn vào màu sắc của ô vuông trên bàn cờ. Một hình vuông có thể được tô màu bằng tính chẵn lẻ của$x+y$. Mỗi bước di chuyển của hiệp sĩ đều thay đổi$x+y$một lượng lẻ vì việc di chuyển làm thay đổi một tọa độ thêm 1 và tọa độ kia thay đổi 2. Việc cộng các thay đổi đó luôn cho một giá trị lẻ. Kết quả là mỗi bước đi của hiệp sĩ sẽ làm thay đổi màu sắc của hình vuông. 

Điều này có nghĩa là đồ thị hiệp sĩ là lưỡng cực. Đường đi có số bước di chuyển lẻ phải kết thúc bằng màu đối diện và đường đi có số bước di chuyển chẵn phải kết thúc bằng cùng màu. Câu hỏi còn lại là liệu mọi hình vuông có màu đối diện đều có thể tiếp cận được hay không. Hiệp sĩ có thể tiếp cận mọi ô vuông trên một bàn cờ vô hạn, vì vậy sự khác biệt về màu sắc là điều kiện duy nhất quan trọng. 

Brute-force hoạt động vì nó tuân theo trực tiếp định nghĩa biểu đồ, nhưng không thành công vì biểu đồ quá lớn. Thuộc tính tô màu nén toàn bộ biểu đồ thành một lần kiểm tra tính chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K)$mỗi trường hợp thử nghiệm, trong đó$K$là số ô vuông được khám phá |$O(K)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính màu của hình vuông bắt đầu bằng cách sử dụng$(x_1+y_1)\bmod 2$. Giá trị chính xác của tổng là không cần thiết, chỉ cần là số lẻ hay số chẵn, bởi vì màu bàn cờ lặp lại sau mỗi hai ô vuông. 
2. Tính màu của ô đích bằng cách sử dụng$(x_2+y_2)\bmod 2$. Điều này mang lại lớp màu mà hiệp sĩ phải hoàn thành. 
3. So sánh hai màu. Nếu chúng khác nhau, hiệp sĩ phải thực hiện một số nước đi lẻ, vì vậy hãy in`Monke`. Nếu chúng giống nhau thì mọi đường đi có thể đều có độ dài chẵn, vì vậy hãy in`Potato`. 

Tại sao nó hoạt động: mỗi nước đi của hiệp sĩ sẽ chuyển đổi tính chẵn lẻ của$x+y$. Sau một lần di chuyển, màu sẽ bị đảo ngược, sau hai lần di chuyển, nó sẽ được khôi phục và điều này tiếp tục cho tất cả các độ dài đường dẫn. Màu đích xác định hoàn toàn tính chẵn lẻ của mọi độ dài đường dẫn có thể có tới hình vuông đó. Vì tất cả các ô vuông đều có thể truy cập được trên bàn hiệp sĩ vô hạn nên các màu đối lập tương ứng chính xác với các vị trí có thể truy cập được trong một số nước đi lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())

        if (x1 + y1) % 2 != (x2 + y2) % 2:
            ans.append("Monke")
        else:
            ans.append("Potato")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được xử lý mỗi lần một trường hợp kiểm thử, giúp duy trì mức sử dụng bộ nhớ không đổi ngay cả khi có nhiều trường hợp. Chương trình chỉ lưu trữ các chuỗi đầu ra trước khi in chúng cùng nhau, tránh chi phí cho nhiều thao tác đầu ra riêng lẻ. 

biểu thức`(x1 + y1) % 2`là toàn bộ cốt lõi của giải pháp. Vì chỉ tính chẵn lẻ mới quan trọng nên các giá trị tọa độ lớn không tạo ra bất kỳ khó khăn nào. Số nguyên Python cũng tránh được vấn đề tràn, mặc dù tổng tối đa có thể có ở đây đã đủ nhỏ đối với các loại số nguyên tiêu chuẩn trong các ngôn ngữ khác. 

Việc so sánh được thực hiện trực tiếp giữa hai màu hình vuông. Không cần tính toán khoảng cách, tạo nước đi hiệp sĩ hoặc xử lý các trường hợp tọa độ đặc biệt vì bảng vô hạn loại bỏ các hiệu ứng biên. 

## Ví dụ đã hoạt động 

Sử dụng các trường hợp mẫu: 

| Bắt đầu | Mục tiêu | Bắt đầu chẵn lẻ | Mục tiêu ngang bằng | Kết quả | 
| --- | --- | --- | --- | --- | 
| (1,1) | (4,5) | 0 | 1 | Khỉ | 
| (100,2) | (9,11) | 0 | 0 | Khoai tây | 
| (1,1) | (1,2) | 0 | 1 | Khỉ | 

Trường hợp đầu tiên chứng minh rằng câu trả lời không phải là việc một hiệp sĩ bỏ đi. Màu sắc khác nhau nên tồn tại một số tuyến đường có độ dài lẻ. 

Trường hợp thứ hai chứng tỏ rằng các hình vuông cùng màu không thể nối với nhau bằng số lẻ nước đi. Mỗi nước đi của hiệp sĩ sẽ lật màu, vì vậy việc quay trở lại cùng một màu cần có số lần lật chẵn. 

Trường hợp thứ ba là trường hợp màu đối diện đơn giản nhất. Quân mã có thể cần vài nước đi, nhưng điều kiện chẵn lẻ đảm bảo sự tồn tại của một đường đi có độ dài lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm chỉ yêu cầu bốn phép cộng và so sánh chẵn lẻ. | 
| Không gian |$O(t)$| Danh sách đầu ra lưu trữ một câu trả lời cho mỗi trường hợp kiểm tra. | 

Với$t$lên đến$10^5$, thời gian chạy tuyến tính dễ dàng nằm trong giới hạn. Thuật toán không phụ thuộc vào kích thước tọa độ hoặc yêu cầu bất kỳ cấu trúc dữ liệu bổ sung nào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())
        if (x1 + y1) % 2 != (x2 + y2) % 2:
            ans.append("Monke")
        else:
            ans.append("Potato")

    return "\n".join(ans)

# provided sample
assert solution("""3
1 1 4 5
100 2 9 11
1 1 1 2
""") == """Monke
Potato
Monke""", "sample"

# minimum-size style case: same square
assert solution("""1
1 1 1 1
""") == "Potato", "same square"

# boundary values
assert solution("""1
1000000 1000000 1 2
""") == "Monke", "large coordinates"

# same color but far apart
assert solution("""1
1 1 1000000 1000000
""") == "Potato", "same parity"

# opposite color adjacent coordinates
assert solution("""1
999999 1000000 1000000 1000000
""") == "Monke", "parity difference"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 4 5`|`Monke`| Kiểm tra xem thuật toán không bị giới hạn ở một nước đi của hiệp sĩ. | 
|`1 1 1 1`|`Potato`| Kiểm tra trường hợp di chuyển bằng 0 trong đó màu hình vuông không thay đổi. | 
|`1000000 1000000 1 2`|`Monke`| Kiểm tra xử lý tọa độ tối đa. | 
|`1 1 1000000 1000000`|`Potato`| Kiểm tra xem các hình vuông ở xa cùng màu có bị từ chối hay không. | 
|`999999 1000000 1000000 1000000`|`Monke`| Kiểm tra trường hợp chênh lệch chẵn lẻ biên. | 

## Vỏ cạnh 

Đối với trường hợp`1 1 1 1`, cả hai hình vuông đều có tính chẵn lẻ$(1+1)\bmod 2=0$. Thuật toán so sánh màu sắc và kết quả đầu ra bằng nhau`Potato`. Việc triển khai bất cẩn giả sử số lần di chuyển bằng 0 được tính là đường dẫn lẻ hợp lệ sẽ in không chính xác`Monke`. 

Đối với trường hợp`1 1 1 3`, cả hai hình vuông cũng có tính chẵn lẻ. Quân mã không thể đến với số nước đi lẻ vì mỗi nước đi sẽ chuyển màu, và số nước đi lẻ không thể trở về màu ban đầu. Thuật toán ngay lập tức phát hiện tính chẵn lẻ bằng nhau và kết quả đầu ra`Potato`. 

Đối với trường hợp`1000000 1000000 1 2`, ô vuông bắt đầu có tính chẵn lẻ$0$, trong khi đích đến có tính chẵn lẻ$1$. Các giá trị lớn, nhưng chỉ bit cuối cùng của tổng tọa độ mới quan trọng. Thuật toán tìm màu sắc và kết quả đầu ra khác nhau`Monke`không phụ thuộc vào kích thước bảng.
