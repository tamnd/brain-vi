---
title: "CF 105674A - \u041a\u0443\u0437\u043d\u0435\u0447\u0438\u043a 2D"
description: "Chúng ta đang làm việc trên một lưới hình chữ nhật có tọa độ tăng dần về bên phải và hướng lên trên. Phần bắt đầu ở ô dưới cùng bên trái, là $(1,1)$ và mục tiêu là đến ô trên cùng bên phải $(n,m)$."
date: "2026-06-22T05:10:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105674
codeforces_index: "A"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f, 1 \u0442\u0443\u0440"
rating: 0
weight: 105674
solve_time_s: 53
verified: true
draft: false
---

[CF 105674A - \u041a\u0443\u0437\u043d\u0435\u0447\u0438\u043a 2D](https://codeforces.com/problemset/problem/105674/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc trên một lưới hình chữ nhật có tọa độ tăng dần về bên phải và hướng lên trên. Phần này bắt đầu ở ô phía dưới bên trái, đó là$(1,1)$, và mục tiêu là đến ô trên cùng bên phải$(n,m)$. 

Trong mỗi lần di chuyển, quân cờ có thể chọn một trong ba hướng: hoàn toàn bên phải, hoàn toàn hướng lên hoặc theo đường chéo lên bên phải. Hạn chế quan trọng là theo hướng đã chọn, nó có thể di chuyển về phía trước theo bất kỳ số ô nào từ 1 đến$k$chỉ trong một lần di chuyển. Vì vậy, một bước di chuyển không phải là một bước di chuyển liền kề một bước mà là một bước “nhảy” có giới hạn dọc theo một trong ba hướng được phép. 

Một cách hữu ích để diễn đạt lại điều này là mỗi nước đi sẽ làm tăng tọa độ x lên một lượng nào đó trong$[0,k]$và độc lập tăng tọa độ y lên một lượng nào đó trong$[0,k]$, ngoại trừ việc cả hai không thể bằng 0 cùng một lúc. Tùy chọn đường chéo chính xác là thứ cho phép cả hai tọa độ tiến lên trong cùng một bước di chuyển. 

Đầu ra là số lần di chuyển tối thiểu cần thiết để biến đổi$(1,1)$vào trong$(n,m)$. 

Những ràng buộc cho phép$n,m,k$lên đến$10^9$, điều này ngay lập tức loại trừ bất kỳ phương pháp truyền tải lưới hoặc lập trình động nào trên tọa độ. Bất kỳ giải pháp nào phụ thuộc vào việc lặp qua các trạng thái tỷ lệ thuận với$n$hoặc$m$sẽ không kết thúc đúng thời hạn. Thậm chí$O(n)$mỗi bài kiểm tra sẽ quá chậm ở quy mô này. 

Đây là bài toán trong đó cấu trúc của chuyển động chi phối mọi thứ: mỗi chuyển động góp phần độc lập vào việc chúng ta có thể tiến bộ bao xa theo cả hai chiều, vì vậy lời giải có thể phụ thuộc vào các giới hạn số học đơn giản hơn là tìm kiếm. 

Một trường hợp tinh vi xuất hiện khi một chiều đã đạt được còn chiều kia thì chưa. Ví dụ, nếu$n=1$Và$m>1$, chuyển động bị hạn chế chỉ ở những bước nhảy lên trên và tương tự đối với trục còn lại. Một trường hợp cạnh khác là khi$n=m=1$, trong đó không cần di chuyển gì cả. Việc triển khai đơn giản luôn tính toán số lần di chuyển dương sẽ trả về không chính xác 1 trong trường hợp này. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực coi mỗi ô lưới là một trạng thái và mỗi bước nhảy hợp lệ là một cạnh sang trạng thái khác. Từ$(x,y)$, chúng ta có thể chuyển sang tất cả$(x+i,y+j)$Ở đâu$0 \le i,j \le k$, loại trừ trường hợp$i=j=0$. Chạy thuật toán đường đi ngắn nhất như BFS sẽ tính toán chính xác số bước di chuyển tối thiểu vì tất cả các cạnh đều có chi phí như nhau. 

Tuy nhiên, số cạnh trên mỗi trạng thái theo thứ tự$k^2$và bản thân lưới có$n \times m$tiểu bang. Ngay cả khi chúng ta giả sử chỉ truy cập các trạng thái có thể truy cập thì cấu trúc phân nhánh trong trường hợp xấu nhất vẫn khiến điều này hoàn toàn không khả thi khi$n,m,k$đang lên đến$10^9$. Không gian trạng thái về mặt khái niệm là rất lớn và không thể hiện thực hóa được. 

Quan sát quan trọng là vấn đề không thực sự phụ thuộc vào các vị trí trung gian. Điều quan trọng chỉ là tổng chuyển vị theo x và y. Mỗi nước đi có thể đóng góp nhiều nhất$k$đến x và nhiều nhất$k$tới y. Việc di chuyển theo đường chéo là điều làm cho những đóng góp này trở nên độc lập trong một bước duy nhất. 

Nếu chúng ta xem xét việc đạt được$(n-1,m-1)$từ$(0,0)$, mỗi nước đi có thể thêm tối đa$k$theo từng tọa độ. Vì thế sau$t$chuyển động thì ta phải có:$$t \cdot k \ge n-1 \quad \text{and} \quad t \cdot k \ge m-1$$Vì thế$t$ít nhất phải bằng mức tối đa của hai mức trần yêu cầu. Câu hỏi còn lại là liệu giới hạn này có thể đạt được hay không. Đó là bởi vì chúng ta luôn có thể nâng cao đồng thời cả hai tọa độ bằng cách di chuyển theo đường chéo cho đến khi thỏa mãn một chiều, sau đó tiếp tục di chuyển một trục ở chiều còn lại. 

Điều này làm giảm vấn đề thành một phép tính số học trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới BFS trên các trạng thái |$O(nm)$|$O(nm)$| Quá chậm | 
| Giới hạn số học (lời giải cuối cùng) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại chuyển động từ$(1,1)$ĐẾN$(n,m)$như một vấn đề chuyển vị. Quãng đường thực tế chúng ta cần đi là$n-1$theo chiều dọc và$m-1$theo chiều ngang. 

1. Tính xem có bao nhiêu lần nhảy kích thước đầy đủ$k$được yêu cầu để che khoảng cách theo chiều dọc$n-1$. Đây là$\lceil (n-1)/k \rceil$. Điều này thể hiện số lần di chuyển tối thiểu cần thiết nếu chúng ta chỉ quan tâm đến tiến trình theo chiều dọc. 
2. Tính cùng một giá trị cho khoảng cách ngang$m-1$, cho$\lceil (m-1)/k \rceil$. Đây là số lần di chuyển tối thiểu nếu chúng ta chỉ quan tâm đến tiến trình theo chiều ngang. 
3. Lấy giá trị lớn nhất của hai giá trị này. Đây là số lần di chuyển tối thiểu cần thiết để thỏa mãn cả hai ràng buộc cùng một lúc. 
4. Xử lý trường hợp ẩn ý trong đó$n=m=1$. Cả hai khoảng cách đều bằng 0, vì vậy cả hai mức trần đều bằng 0 và mức tối đa vẫn bằng 0. 

Lý do tối đa hoạt động là vì mọi chuyển động đều có thể đóng góp vào cả hai tọa độ cùng một lúc. Vì vậy, yếu tố hạn chế không phải là chuyển động tổng thể mà là tốc độ tiến bộ cần thiết chậm hơn. 

### Tại sao nó hoạt động 

Mỗi lần di chuyển sẽ tăng tọa độ x tối đa$k$và tọa độ y nhiều nhất là$k$. Vì thế sau$t$di chuyển, cả hai tọa độ được giới hạn bởi$t \cdot k$về mức độ tiến bộ mà họ có thể tích lũy được ngay từ đầu. Nếu một trong hai yêu cầu tọa độ vượt quá giới hạn này thì không thể đạt được mục tiêu trong$t$di chuyển. 

Ngược lại, nếu$t$thỏa mãn cả hai giới hạn, chúng ta có thể xây dựng một chuỗi các bước di chuyển trong đó chúng ta sử dụng các bước nhảy chéo để tiến cả hai tọa độ cùng nhau cho đến khi hết một chiều, sau đó tiếp tục di chuyển dọc theo trục còn lại. Điều này đảm bảo tính khả thi chính xác ở giới hạn tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    dx = n - 1
    dy = m - 1

    tx = (dx + k - 1) // k
    ty = (dy + k - 1) // k

    print(max(tx, ty))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Trừ 1 từ cả hai tọa độ sẽ chuyển vấn đề từ lập chỉ mục dựa trên ô sang chuyển vị thuần túy. Việc chia trần được thực hiện bằng cách sử dụng số học số nguyên bằng cách thêm$k-1$trước khi chia. 

Một lỗi phổ biến là quên phép biến đổi từng cái một từ$(n,m)$ĐẾN$(n-1,m-1)$. Không có điều này, câu trả lời sẽ trở nên sai bất cứ khi nào$n$hoặc$m$chính xác là chia hết cho$k$hoặc khi sự bắt đầu bằng sự kết thúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 8 5
```Chúng tôi tính toán chuyển vị như$dx=8$,$dy=7$. 

| Bước | dx | nhuộm | tx = ceil(dx/k) | ty = ceil(dy/k) | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 8 | 7 | - | - | - | 
| calc | 8 | 7 | 2 | 2 | 2 | 

Cả hai chiều đều yêu cầu hai lần di chuyển khi được nhóm thành các khối có kích thước 5. Kết quả là 2 vì chuyển động theo đường chéo cho phép cả hai tọa độ cùng tiến lên, do đó không chiều nào lấn át chiều kia. 

### Ví dụ 2 

đầu vào:```
2 2 1
```Đây$dx=1$,$dy=1$. 

| Bước | dx | nhuộm | tx | ty | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | - | - | - | 
| calc | 1 | 1 | 1 | 1 | 1 | 

Mỗi lần di chuyển chỉ tiến lên một ô theo mỗi hướng vì$k=1$. Một bước di chuyển chéo là đủ để tiếp cận mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép các giá trị lên đến$10^9$, vì vậy mọi giải pháp đều phải tránh lặp lại tọa độ. Một giải pháp số học theo thời gian không đổi phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert solve_io("9 8 5\n") == "2"
assert solve_io("2 2 1\n") == "1"

# minimum case
assert solve_io("1 1 1\n") == "0"

# single row
assert solve_io("1 10 3\n") == "3"

# single column
assert solve_io("10 1 3\n") == "3"

# large k covers both in one move
assert solve_io("100 100 200\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | Bắt đầu bằng kết thúc | 
| 1 10 3 | 3 | Chuyển động trục đơn | 
| 10 1 3 | 3 | Trường hợp trục đơn đối xứng | 
| 100 100 200 | 1 | Bước nhảy lớn bao gồm cả hai trục | 

## Vỏ cạnh 

Khi nào$n=m=1$, độ dịch chuyển bằng 0 theo cả hai hướng. Các giá trị tính toán trở thành$\lceil 0/k \rceil = 0$, do đó mức tối đa bằng 0 và không cần di chuyển. Việc triển khai có lỗi mà quên bước trừ có thể coi việc này không chính xác là yêu cầu một lần di chuyển, nhưng công thức dịch chuyển sẽ tránh được điều đó. 

Khi một chiều lớn hơn đáng kể so với chiều kia, thuật toán sẽ tự nhiên chọn mức trần lớn hơn. Ví dụ, nếu$n$lớn và$m=1$, sau đó$dy=0$Và$\lceil 0/k \rceil=0$. Câu trả lời quy giản hoàn toàn về tiến trình theo chiều dọc, phù hợp với ràng buộc là không cần tiến trình đi lên.
