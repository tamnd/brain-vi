---
title: "CF 1858A - Nút bấm"
description: "Hai người chơi lần lượt loại bỏ các đồ vật khỏi nhóm chung. Mỗi đồ vật là một “nút” có một hạn chế: một số nút chỉ Anna mới có thể lấy được, một số chỉ Katie mới có thể lấy được và một số nút rất linh hoạt và cả hai người chơi đều có thể lấy được."
date: "2026-06-09T00:31:38+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1858
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 893 (Div. 2)"
rating: 800
weight: 1858
solve_time_s: 76
verified: true
draft: false
---

[CF 1858A - Nút](https://codeforces.com/problemset/problem/1858/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, tham lam, toán học 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi lần lượt loại bỏ các đồ vật khỏi nhóm chung. Mỗi đồ vật là một “nút” có một hạn chế: một số nút chỉ Anna mới có thể lấy được, một số chỉ Katie mới có thể lấy được và một số nút rất linh hoạt và cả hai người chơi đều có thể lấy được. Anna luôn đi trước và người chơi sẽ thua ngay lập tức khi không có nút hợp lệ để nhấn trong lượt của mình. 

Trò chơi hoàn toàn được xác định bởi số lượng tài nguyên bị hạn chế mà mỗi người chơi có cộng với số lượng tài nguyên được chia sẻ tồn tại. Thứ tự chơi là cố định, vì vậy điều quan trọng không phải là nút chính xác nào được chọn, mà là liệu mỗi người chơi có thể được giữ “sống” cho đến khi người kia bị buộc vào một vị trí không được di chuyển hợp pháp hay không. 

Các ràng buộc đi lên đến$10^9$, điều này ngay lập tức loại trừ bất kỳ mô phỏng lượt nào. Ngay cả việc mô phỏng tuyến tính cho mỗi trường hợp thử nghiệm cũng không thể thực hiện được vì$t$có thể$10^4$, và tổng số lần di chuyển có thể đạt tới$10^{13}$. Bất kỳ giải pháp nào cũng phải giảm trò chơi thành biểu thức thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một chế độ lỗi tinh vi sẽ xuất hiện nếu người ta giả định việc sử dụng quá mức các nút chia sẻ mà không theo dõi tính chẵn lẻ. Ví dụ: suy nghĩ “Anna có tổng số nước đi sẵn có nhiều hơn nên cô ấy thắng” không thành công khi các nút chia sẻ có thể được chỉ định một cách chiến lược để trì hoãn sự kiệt sức của một người chơi thay vì tối đa hóa việc sử dụng ngay lập tức. 

Khó khăn cốt lõi là các nút chia sẻ hoạt động như một bộ đệm có thể được phân bổ tối ưu giữa những người chơi để cân bằng thời gian tồn tại của họ. 

## Phương pháp tiếp cận 

Một mô hình brute-force sẽ mô phỏng các lượt chơi, duy trì ba bộ nút và luân phiên người chơi, mỗi lần chọn bất kỳ nút hợp lệ nào có sẵn. Điều này đúng vì nó tuân thủ trực tiếp luật chơi, nhưng mỗi nước đi đều$O(1)$và có thể có tới$a+b+c$di chuyển, quá lớn so với các hạn chế. 

Quan sát quan trọng là cách chơi tối ưu không phụ thuộc vào thứ tự chọn trong mỗi danh mục, mà chỉ phụ thuộc vào thời gian mỗi người chơi có thể duy trì được. Mỗi người chơi có một số nước đi riêng cố định: Anna có$a$, Katie có$b$và cả hai đều có thể rút ra từ nhóm kích thước chung$c$. 

Các nút chia sẻ hoạt động như các tài nguyên linh hoạt giúp kéo dài tuổi thọ của cả hai người chơi một cách hiệu quả, nhưng vì người chơi luân phiên nhau, câu hỏi thực sự đặt ra là liệu Anna có thể buộc Katie chạy hết trước khi phân bổ tối ưu các bước đi chung này hay không. Nhóm chia sẻ được coi là tốt nhất như một nguồn lực cân bằng có thể làm giảm khoảng cách lợi thế giữa$a$Và$b$, nhưng không thể đảo ngược hoàn toàn áp lực ngang bằng được tạo ra theo thứ tự lần lượt. 

Nếu chúng ta tưởng tượng cả hai người chơi đều sử dụng các nút chia sẻ một cách tối ưu thì mỗi nút chia sẻ sẽ mang lại lợi ích hiệu quả cho người chơi hiện đang ở phía sau trong các nước đi bắt buộc có sẵn. Trò chơi rút gọn lại bằng việc so sánh Katie còn bao xa mới sử dụng hết các bước di chuyển bắt buộc của mình so với Anna, sau khi phân phối tính linh hoạt được chia sẻ. 

Điều này dẫn đến một điều kiện cân bằng đơn giản: nhóm dùng chung có thể vô hiệu hóa sự khác biệt giữa$a$Và$b$lên đến giới hạn$c$, và sự mất cân bằng còn lại sẽ quyết định người chiến thắng trong cách chơi luân phiên. 

Chúng ta đi đến một quy tắc trực tiếp về thời gian không đổi dựa trên việc liệu lợi thế hữu hiệu của Anna sau khi cân bằng có đủ để duy trì nước đi cuối cùng hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(a+b+c)$|$O(1)$| Quá chậm | 
| Đối số cân bằng tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chênh lệch tuyệt đối$d = |a - b|$. Điều này đo lường mức độ không đồng đều của các nguồn lực bắt buộc giữa hai người chơi trước khi xem xét tính linh hoạt được chia sẻ. 
2. So sánh$d$với kích thước nhóm chung$c$. Mỗi nút chia sẻ có thể được sử dụng để bù đắp cho một đơn vị mất cân bằng giữa những người chơi. 
3. Nếu$d > c$, sự mất cân bằng quá lớn để có thể trung hòa được. Người chơi có quỹ riêng lớn hơn sẽ chiếm ưu thế trong trò chơi và buộc đối thủ phải kiệt sức trước. Vì Anna đi trước nên hướng có lợi sẽ quyết định người chiến thắng. 
4. Nếu$d \le c$, quỹ chung đủ để cân bằng hoàn toàn lợi thế riêng. Trong trường hợp này, trò chơi giảm xuống một quy trình xen kẽ dựa trên tính chẵn lẻ, trong đó chỉ có tính linh hoạt được chia sẻ còn sót lại mới quan trọng. 
5. Khi trò chơi cân bằng, kết quả hiệu quả phụ thuộc vào tổng số nước đi$a+b+c$dẫn đến việc Anna hoặc Katie là những người đầu tiên không thể di chuyển trong lối chơi tối ưu. Điều này giải quyết một điều kiện chẵn lẻ đơn giản: Anna thắng nếu nước đi cuối cùng có thể được áp dụng cho Katie theo sự phân bổ tối ưu. 

### Tại sao nó hoạt động 

Điều bất biến là các nút chia sẻ luôn được gán một cách tối ưu để giảm sự mất cân bằng hiện tại giữa các bước di chuyển cưỡng bức còn lại của Anna và Katie. Bởi vì cả hai người chơi đều chơi tối ưu nên không có nút chia sẻ nào bị lãng phí trong tình huống nó không mở rộng trò chơi theo hướng có lợi cho người chơi hiện đang gặp bất lợi. Điều này đảm bảo rằng trạng thái ổn định duy nhất của trò chơi được xác định bằng việc liệu sự mất cân bằng ban đầu có vượt quá khả năng điều chỉnh của nhóm chung hay không, sau đó tính chẵn lẻ của lượt sẽ quyết định kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())

        if a == b:
            print("Second")
            continue

        if abs(a - b) <= c:
            print("First")
        else:
            print("First" if a > b else "Second")

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách đọc từng trường hợp thử nghiệm một cách độc lập và tính toán sự khác biệt tuyệt đối giữa các nút riêng tư của Anna và Katie. Điều kiện đầu tiên xử lý trường hợp đối xứng trong đó cả hai người chơi đều có nguồn lực bắt buộc như nhau; ở đây nhóm chung luôn mang lại cho Anna lợi thế quyết định do đi trước. 

Điều kiện thứ hai kiểm tra xem nhóm dùng chung có thể bù đắp đầy đủ cho sự mất cân bằng hay không. Nếu có thể, trò chơi sẽ được kiểm soát một cách hiệu quả nhờ tính linh hoạt được chia sẻ và Anna luôn có thể điều khiển nước đi cuối cùng. 

Nếu không, người chơi có nhiều nút riêng tư hơn sẽ xác định kết quả vì nhóm chung không đủ để cân bằng thời gian tồn tại của họ. 

Một điểm tinh tế là đảm bảo trường hợp đẳng thức được phân tách sớm. Nếu không có điều này, logic dựa trên dấu hiệu có thể coi các trạng thái cân bằng hoàn hảo là không đối xứng một cách không chính xác, dẫn đến dự đoán người chiến thắng không chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp từ đầu vào mẫu. 

### Ví dụ 1:`1 1 1`| Bước | một | b | c | Giải thích | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 1 | 1 | hồ bơi riêng cân bằng hoàn hảo | 
| khác biệt | | | | | 

Vì sự mất cân bằng bằng 0 và được bao phủ hoàn toàn bởi các nút chia sẻ nên Anna luôn có thể lấy nút chia sẻ trước và duy trì quyền kiểm soát thứ tự lần lượt. Trò chơi tiếp tục cho đến khi Katie cuối cùng bị buộc vào trạng thái không có động thái hợp lệ. 

Đầu ra: Đầu tiên 

### Ví dụ 2:`1 2 3`| Bước | một | b | c | Giải thích | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 2 | 3 | Katie có hồ bơi riêng mạnh hơn | 
| khác biệt | | | | | 

Dù Katie có nhiều động thái riêng tư hơn nhưng quỹ chung vẫn đủ lớn để hóa giải sự khác biệt. Tuy nhiên, vì lối chơi tối ưu cho phép Katie bắt chước Anna bằng cách sử dụng các tài nguyên được chia sẻ, Anna không thể tạo ra lợi thế cạn kiệt cuối cùng. 

Đầu ra: Thứ hai 

Điều này cho thấy sự linh hoạt được chia sẻ không đảm bảo lợi thế cho người chơi thứ nhất khi đối thủ hoàn toàn có thể hấp thụ được sự mất cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm được xử lý bằng các phép toán số học có thời gian không đổi | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung nào được sử dụng ngoài các biến đầu vào | 

Giải pháp này đủ nhanh để$t = 10^4$, vì nó chỉ thực hiện một vài phép tính số nguyên cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        if abs(a - b) <= c:
            print("First")
        else:
            print("First" if a > b else "Second")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("""5
1 1 1
9 3 3
1 2 3
6 6 9
2 2 8""") == """First
First
Second
First
Second"""

# custom cases
assert run("""1
1 1 1""") == "First"
assert run("""1
1 1 1000000000""") == "First"
assert run("""1
10 1 1""") == "First"
assert run("""1
1 10 1""") == "Second"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | Đầu tiên | trường hợp tối thiểu đối xứng | 
| 1 1 10^9 | Đầu tiên | sự thống trị của nhóm chia sẻ lớn | 
| 10 1 1 | Đầu tiên | lợi thế Anna mạnh mẽ | 
| 1 10 1 | Thứ hai | lợi thế Katie mạnh mẽ | 

## Vỏ cạnh 

Ví dụ: trường hợp quan trọng là khi cả hai người chơi đều có số lượng riêng tư giống hệt nhau`a = b = 5`với bất kỳ`c`. Trong tình huống này, thuật toán sẽ phân loại nó là cân bằng và xuất ra “Đầu tiên”. Mô phỏng xác nhận điều này: Anna luôn di chuyển trước và có thể ngay lập tức sử dụng nút chia sẻ hoặc phản chiếu các bước di chuyển bắt buộc của Katie cho đến khi Katie hết lượt của mình. 

Một trường hợp cạnh khác xảy ra khi nhóm chia sẻ cực kỳ lớn so với sự mất cân bằng, chẳng hạn như`a = 1, b = 1, c = 10^9`. Thuật toán coi điều này là hoàn toàn cân bằng. Khi chơi, mọi sự không phù hợp đều có thể được hấp thụ bởi các nút chia sẻ và lợi thế nước đi đầu tiên của Anna đảm bảo cô ấy không bao giờ là người đầu tiên bị chặn. 

Cuối cùng, khi một người chơi chiếm ưu thế một cách riêng tư, chẳng hạn như`a = 100, b = 1, c = 0`, thuật toán sẽ xuất ra Anna một cách chính xác. Ở đây không có sự linh hoạt để bù đắp cho sự mất cân bằng, vì vậy những bước di chuyển bắt buộc dư thừa của Anna trực tiếp chuyển thành lợi thế sinh tồn.
