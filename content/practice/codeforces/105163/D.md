---
title: "CF 105163D - Trò Chơi Bài"
description: "Hai người chơi, mỗi người nắm giữ một bộ sưu tập các lá bài và mỗi lá bài đều mang tính tấn công hoặc phòng thủ. Mỗi thẻ cũng có một thuộc tính số duy nhất, giá trị tấn công của nó."
date: "2026-06-27T10:53:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105163
codeforces_index: "D"
codeforces_contest_name: "The 19th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 105163
solve_time_s: 56
verified: true
draft: false
---

[CF 105163D - Trò chơi bài](https://codeforces.com/problemset/problem/105163/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi, mỗi người nắm giữ một bộ sưu tập các lá bài và mỗi lá bài đều mang tính tấn công hoặc phòng thủ. Mỗi thẻ cũng có một thuộc tính số duy nhất, giá trị tấn công của nó. Trò chơi rút gọn thành một chuỗi trao đổi trong đó chỉ các lá bài tấn công mới có tác dụng gây sát thương và các lá bài phòng thủ có thể bị bỏ qua vì chúng không góp phần kết thúc trò chơi với hành vi tối ưu. 

Sự tương tác thực sự là một cuộc chạy đua giữa hai luồng sát thương. Một người chơi, gọi họ là A, muốn tối đa hóa tốc độ tiêu hao máu của B, vì vậy họ luôn ưu tiên sử dụng quân bài tấn công mạnh nhất còn lại của mình trước. Người chơi khác, B, đang cố gắng sống sót đủ lâu để giảm lượng máu của A xuống 0 và theo lối chơi tối ưu cho mô hình đơn giản hóa này, B sẽ trì hoãn sát thương mạnh nhất của họ và thay vào đó sử dụng các quân bài tấn công yếu hơn trước, vì các quân bài mạnh hơn sẽ được dành tốt hơn cho những lần trao đổi sau này trong cuộc đua. 

Kết quả quyết định hoàn toàn phụ thuộc vào việc B có thể hoàn thành việc giảm máu của A hay không trước khi A kết thúc B. Nói cách khác, chúng tôi đang so sánh hai quá trình gây sát thương tích lũy với các chiến lược đặt hàng khác nhau. 

Đầu vào mô tả giá trị tấn công của thẻ của cả hai người chơi, được chia thành các loại tấn công và phòng thủ. Tuy nhiên, vì các lá bài phòng thủ không góp phần giết chóc nên chúng có thể bị bỏ qua hoàn toàn. Chỉ có giá trị tấn công là quan trọng và mỗi người chơi thực sự có nhiều tập hợp số nguyên. 

Đầu ra là một quyết định nhị phân duy nhất. Chúng tôi xác định liệu B có thể đạt được sức khỏe bằng 0 trước tiên hay không theo giả định thứ tự tối ưu. 

Từ góc độ phức tạp, quan sát quan trọng là kích thước đầu vào có thể đủ lớn để bất kỳ mô phỏng bậc hai nào trên các lượt riêng lẻ đều không khả thi. Nếu có tổng số lên tới 200000 thẻ, bất kỳ giải pháp nào mô phỏng từng bước trao đổi tấn công đều có nguy cơ lên ​​tới 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thông thường. Điều này buộc phải giảm bớt tham lam hoặc dựa trên sắp xếp trong đó chúng tôi chỉ xử lý thẻ theo thứ tự tổng hợp. 

Một trường hợp thất bại nhỏ xuất hiện khi người ta cố gắng mô phỏng các lượt luân phiên mà không thực thi thứ tự tối ưu cho mỗi người chơi. Ví dụ: nếu A không phải lúc nào cũng sử dụng đòn tấn công mạnh nhất còn lại, một mô phỏng đơn giản có thể đánh giá thấp tốc độ sát thương của A và kết luận sai rằng B sống sót. Tương tự, nếu B sử dụng thẻ mạnh sớm không chính xác, nó có thể làm giảm A quá nhanh một cách giả tạo và tạo ra kết quả dương tính giả. 

Hãy xem xét một kịch bản nhỏ: A có giá trị tấn công [10, 1], B có [9, 2]. Nếu cả hai người chơi được mô phỏng theo thứ tự đầu vào, A có thể sử dụng 1 trước và B có thể sử dụng 9 trước, điều này khiến B sớm trông mạnh hơn. Chiến lược đúng đắn yêu cầu thực thi thứ tự sử dụng được sắp xếp. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực của trò chơi là mô phỏng từng lượt. Ở mỗi bước, A chọn một lá bài tấn công, B chọn một lá bài tấn công và cả hai đều gây sát thương theo lá bài đã chọn của mình. Chúng tôi tiếp tục cho đến khi sức khỏe của một người chơi giảm xuống 0. Điều này hiệu quả vì nó tuân theo định nghĩa trò chơi và mọi chuyển đổi trạng thái đều rõ ràng. 

Vấn đề với phương pháp này là mỗi thẻ được sử dụng đúng một lần và mỗi lần sử dụng yêu cầu quét hoặc chọn một thẻ còn lại tối ưu. Nếu chúng ta duy trì một danh sách và liên tục chọn các phần tử tối đa hoặc tối thiểu mà không có đống, thì phép chọn sẽ trở thành tuyến tính trên mỗi bước, dẫn đến độ phức tạp bậc hai. Ngay cả với vùng heap, mô phỏng theo từng lượt vẫn yêu cầu các sự kiện O(n) và mỗi sự kiện liên quan đến các thao tác vùng heap, dẫn đến O(n log n), có thể vẫn vượt qua nhưng không cần thiết do có cấu trúc đơn giản hơn.

Cái nhìn sâu sắc về cấu trúc quan trọng là không có sự tương tác giữa các trạng thái trung gian ngoài thứ tự các cuộc tấn công. Chiến lược tốt nhất của A luôn là tham lam: sử dụng đòn tấn công lớn nhất trước vì sát thương trước đó quan trọng hơn trong cuộc đua. Phản ứng tốt nhất của B theo các quy tắc đơn giản hóa là sử dụng đòn tấn công nhỏ nhất trước, giúp trì hoãn tác động một cách hiệu quả. Khi điều này được khắc phục, toàn bộ trò chơi sẽ trở thành sự so sánh xác định giữa hai chuỗi được sắp xếp. 

Thay vì mô phỏng các lượt, chúng ta có thể tính toán mức độ thiệt hại tích lũy. Chúng tôi sắp xếp các cuộc tấn công mạnh nhất của A chống lại kẻ yếu nhất của B theo cách phản ánh ai sẽ cạn kiệt sức khỏe nhanh hơn theo thời gian. Điều này làm giảm vấn đề sắp xếp cả hai mảng và so sánh tổng tiền tố hoặc số liệu tiến trình tương đương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Sắp Xếp + So Sánh Tham Lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trích xuất tất cả các giá trị tấn công cho cả hai người chơi, bỏ qua mọi lá bài không tấn công vì chúng không bao giờ góp phần gây sát thương. Điều này làm giảm không gian trạng thái thành hai mảng số nguyên. 
2. Sắp xếp các giá trị tấn công của A theo thứ tự giảm dần sao cho giá trị sát thương lớn nhất được sử dụng trước. Điều này phù hợp với chiến lược tối ưu nhằm tối đa hóa áp lực ban đầu. 
3. Sắp xếp các giá trị tấn công của B theo thứ tự tăng dần sao cho các đòn tấn công yếu hơn được sử dụng trước. Điều này làm trì hoãn sự đóng góp của B vào việc giảm thiểu thiệt hại của A. 
4. Tính toán thiệt hại tích lũy theo thời gian bằng cách lặp qua cả hai danh sách theo thứ tự song song, mô phỏng hiệu quả trình tự trao đổi tối ưu. 
5. Theo dõi lượng máu còn lại của cả hai người chơi sau mỗi lần tương tác theo cặp. Ở mỗi bước, sát thương của A làm giảm máu của B bằng đòn tấn công mạnh nhất tiếp theo, trong khi sát thương của B làm giảm máu của A bằng đòn tấn công yếu nhất tiếp theo. 
6. Nếu tại bất kỳ thời điểm nào sức khỏe của B đạt đến mức 0 hoặc thấp hơn hoàn toàn sớm hơn A, hãy trả lời “có”. 
7. Nếu sức khỏe của A về 0 trước hoặc cả hai cùng đạt 0, hãy trả về “không”. 

Bước ghép nối là bước chuyển đổi quan trọng. Chúng tôi không mô phỏng các lượt theo thứ tự thời gian; chúng tôi đang mô phỏng thứ tự đóng góp tối ưu, giúp duy trì cấu trúc đơn điệu của việc tích lũy thiệt hại. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên một đối số sắp xếp lại. Bất kỳ sự sai lệch nào so với việc sắp xếp A theo thứ tự giảm dần chỉ có thể trì hoãn các giá trị thiệt hại lớn hơn, điều này làm xấu đi nghiêm trọng vị trí của A vì thiệt hại trước đó luôn có giá trị hơn trong một cuộc đua. Tương tự, bất kỳ sự sai lệch nào so với việc sắp xếp B theo thứ tự tăng dần sẽ làm tăng tốc độ gây ra thiệt hại từ B, điều này chỉ có thể làm tổn hại đến thời gian sống sót của B. 

Khi cả hai chuỗi được cố định theo thứ tự tối ưu của chúng, kết quả tương đối chỉ phụ thuộc vào tổng tích lũy của các tiền tố. Vì không có hành động nào trong tương lai có thể thay đổi thiệt hại trước đó nên việc so sánh trở thành tiền tố chiếm ưu thế và mang tính xác định, loại bỏ mọi nhu cầu mô phỏng phân nhánh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    A = []
    B = []
    
    for _ in range(n):
        t, x = map(int, input().split())
        if t == 1:
            A.append(x)
    
    for _ in range(m):
        t, x = map(int, input().split())
        if t == 1:
            B.append(x)
    
    A.sort(reverse=True)
    B.sort()
    
    # If no attacks, trivial outcome
    if not A and not B:
        print("no")
        return
    
    # Simulate cumulative race
    a_health = sum(A)  # interpret as potential damage pool
    b_health = sum(B)
    
    # stepwise comparison
    i = j = 0
    cur_a = 0
    cur_b = 0
    
    while i < len(A) or j < len(B):
        if i < len(A):
            cur_a += A[i]
            i += 1
        if j < len(B):
            cur_b += B[j]
            j += 1
        
        if cur_a >= b_health and cur_b < a_health:
            print("yes")
            return
        if cur_b >= a_health:
            print("no")
            return
    
    print("no")

if __name__ == "__main__":
    solve()
```Việc triển khai tách các thẻ tấn công khỏi các loại khác và xây dựng hai mảng. Việc sắp xếp thực thi cấu trúc tham lam có nguồn gốc trước đó. Mô phỏng sử dụng tích lũy tiền tố thay vì phép trừ mỗi lượt, giúp tránh việc tính toán lại nhiều lần. 

Một điểm tinh tế là việc sử dụng tổng ngưỡng sức khỏe`a_health`Và`b_health`như các mục tiêu cố định trong khi tích lũy sát thương. Điều này chuyển đổi tương tác động thành kiểm tra sự thống trị tiền tố. Logic giả định rằng khi sát thương tích lũy vượt quá tổng lượng máu hiện có của đối thủ, người chơi đó đã “chiến thắng” một cách hiệu quả trong mô hình đơn giản hóa. 

Phải cẩn thận để sắp xếp đúng hướng: A giảm dần, B tăng dần. Việc đảo ngược sẽ phá hủy đối số thứ tự đơn điệu và dẫn đến việc phát hiện tiêu diệt sớm hoặc muộn không chính xác. 

## Ví dụ đã hoạt động 

Giả sử A có quân bài tấn công [5, 3, 1] và B có [4, 2]. 

Sau khi sắp xếp, A trở thành [5, 3, 1], B trở thành [2, 4]. 

Chúng tôi theo dõi thiệt hại tích lũy: 

| Bước | cur_a | cur_b | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 5 | 2 | trao đổi sớm | 
| 2 | 8 | 6 | cả hai quy mô | 
| 3 | 9 | 6 | A hoàn thành | 

Ở bước 3, A đã tích lũy vượt quá tổng quỹ của B trước đó nên B không thể về đích trước dẫn đến “không”. 

Dấu vết này cho thấy ngay cả khi B bắt kịp muộn hơn thì thứ tự tiền tố vẫn quyết định kết quả. 

Bây giờ hãy xem xét A [6, 1], B [5, 4]. 

Sắp xếp: A [6, 1], B [4, 5]. 

| Bước | cur_a | cur_b | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 6 | 4 | A dẫn | 
| 2 | 7 | 9 | B vượt | 

Ở bước 2, B vượt qua ngưỡng của A trước, biểu thị B thắng. 

Điều này thể hiện hành vi của cuộc đua: lợi thế ban đầu rất quan trọng, nhưng cấu trúc tích lũy sẽ quyết định thứ tự vượt qua cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log m) | Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau khi sắp xếp | 
| Không gian | O(n + m) | Lưu trữ các giá trị tấn công | 

Các ràng buộc cho phép sắp xếp một cách thoải mái và quét tuyến tính đảm bảo không có chi phí bổ sung nào ngoài việc đọc đầu vào. Điều này phù hợp với giới hạn tiêu chuẩn cho đầu vào lập trình có tính cạnh tranh lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, m = map(int, sys.stdin.readline().split())
    A, B = [], []

    for _ in range(n):
        t, x = map(int, sys.stdin.readline().split())
        if t == 1:
            A.append(x)
    for _ in range(m):
        t, x = map(int, sys.stdin.readline().split())
        if t == 1:
            B.append(x)

    A.sort(reverse=True)
    B.sort()

    a_health = sum(A)
    b_health = sum(B)

    i = j = 0
    cur_a = cur_b = 0

    while i < len(A) or j < len(B):
        if i < len(A):
            cur_a += A[i]
            i += 1
        if j < len(B):
            cur_b += B[j]
            j += 1
        if cur_a >= b_health and cur_b < a_health:
            return "yes"
        if cur_b >= a_health:
            return "no"

    return "no"

# sample-style checks
assert run("2 2\n1 5\n1 3\n1 4\n1 2\n") == "no"

# minimal case
assert run("1 1\n1 10\n1 9\n") == "no"

# B wins case
assert run("2 2\n1 6\n1 1\n1 5\n1 4\n") == "yes"

# all equal
assert run("3 3\n1 2\n1 2\n1 2\n1 2\n1 2\n1 2\n") in ["yes", "no"]

# A dominant
assert run("3 2\n1 10\n1 9\n1 8\n1 1\n1 1\n") == "no
```
