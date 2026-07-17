---
title: "CF 103447H - Logic để làm gì?"
description: "Mỗi truy vấn đưa ra hai chuỗi số nguyên và một tham số $k$. Hoạt động được phép là chọn vị trí bắt đầu $a$ và hoán đổi hai khối có độ dài liên tiếp $k$: đoạn $S[a..a+k-1]$ được hoán đổi với $S[a+k..a+2k-1]$."
date: "2026-07-03T07:31:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103447
codeforces_index: "H"
codeforces_contest_name: "The 2021 China Collegiate Programming Contest (Harbin)"
rating: 0
weight: 103447
solve_time_s: 34
verified: true
draft: false
---

[CF 103447H - Logic để làm gì?](https://codeforces.com/problemset/problem/103447/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn đưa ra hai chuỗi số nguyên và một tham số$k$. Thao tác được phép là chọn vị trí bắt đầu$a$và hoán đổi hai khối chiều dài liên tiếp$k$: đoạn$S[a..a+k-1]$được trao đổi với$S[a+k..a+2k-1]$. Thao tác này có thể được lặp lại bao nhiêu lần tùy ý và câu hỏi đặt ra là liệu một chuỗi có thể được chuyển đổi thành chuỗi khác bằng cách sử dụng các hoán đổi này hay không. 

Khó khăn chính là việc hoán đổi không phải là sự hoán đổi liền kề đơn giản của các phần tử đơn lẻ, mà là sự trao đổi có cấu trúc của các khối và các khối này chồng chéo lên nhau theo các lựa chọn khác nhau của$a$. Các ràng buộc cho phép lên đến$10^5$tổng số phần tử trên tất cả các truy vấn, do đó, bất kỳ giải pháp nào mô phỏng các phép biến đổi từng bước đều ngay lập tức quá chậm. Ngay cả một truy vấn cũng có thể có độ dài$10^5$, do đó, bất cứ điều gì tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi truy vấn sẽ không tồn tại. 

Một cạm bẫy tinh vi xuất hiện khi người ta giả định rằng thao tác hoạt động giống như các hoán vị tùy ý của các khối có kích thước$k$. Bản thân điều đó là chưa đủ vì các khối chồng chéo và tương tác không độc lập ở cấp độ khối. 

Ví dụ, nếu$k = 2$, thao tác hoán đổi các cặp phần tử trong cửa sổ trượt có độ dài 4. Một cách giải thích đơn giản có thể gợi ý rằng chúng ta chỉ có thể hoán vị các cặp, nhưng trên thực tế, các thao tác lặp lại cho phép trộn sâu hơn bị ràng buộc bởi cấu trúc vị trí. 

Chế độ lỗi thứ hai xuất hiện khi các chuỗi có nhiều tập hợp giống hệt nhau nhưng khác nhau về cấu trúc so với$k$. Ví dụ: hai mảng có cùng giá trị trên toàn cầu vẫn không thể chuyển đổi nếu các giá trị bị căn chỉnh sai trong quá trình phân rã cấu trúc do$k$. 

Do đó, nhiệm vụ là xác định cấu trúc bất biến được bảo toàn bởi phép toán. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng mô hình hóa trình tự và áp dụng tất cả các hoạt động có thể, khám phá các trạng thái có thể tiếp cận. Mỗi thao tác đều tác động$2k$các phần tử và có$O(n)$sự lựa chọn của$a$, do đó, ngay cả một lớp thăm dò cũng phân nhánh rất nhiều. Không gian trạng thái là các hoán vị lên tới$10^5$yếu tố, làm cho điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là ngừng suy nghĩ về các khối liền kề và thay vào đó hãy theo dõi cách các chỉ số di chuyển. Khi chúng ta áp dụng thao tác tại vị trí$a$, mọi chỉ số trong$[a, a+k-1]$hoán đổi với chỉ số tương ứng trong$[a+k, a+2k-1]$. Điều đó có nghĩa là vị trí$i$chỉ tương tác với vị trí$i+k$và không bao giờ với bất kỳ vị trí nào có chỉ số khác nhau bởi thứ gì đó không chia hết cho$k$. 

Điều này tạo ra sự phân rã cứng nhắc: các chỉ số được chia thành các chuỗi độc lập dựa trên modulo giá trị của chúng.$k$. Trong mỗi chuỗi, các hoán đổi lặp đi lặp lại có độ dài chồng chéo-$k$khối cho phép hoán vị tùy ý. Theo trực quan, thao tác này cho phép bạn tạo bong bóng các phần tử dọc theo từng lớp chỉ mục dư lượng và sự chồng lấp đảm bảo có thể sắp xếp lại đầy đủ bên trong mỗi lớp. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem liệu với mỗi lớp dư lượng modulo$k$, tập hợp nhiều giá trị xuất hiện tại các vị trí đó giống hệt nhau trong cả hai chuỗi. 

Khi cấu trúc này được nhận dạng, giải pháp sẽ trở thành sắp xếp hoặc khớp tần số trong mỗi lớp dư lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân hủy lớp dư lượng |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân chia chỉ số của từng dãy thành$k$nhóm theo modulo chỉ số của họ$k$. Bước này phản ánh thực tế là các hoán đổi không bao giờ di chuyển các phần tử qua các lớp dư lượng khác nhau, vì vậy các nhóm này là các vũ trụ độc lập. 
2. Đối với từng loại dư lượng$r$, thu thập tất cả các giá trị$S[i]$như vậy$i \bmod k = r$, và làm tương tự cho$T$. Tính đúng đắn của việc phân nhóm này xuất phát từ việc theo dõi rằng mọi hoạt động được phép đều bảo toàn phần dư của mọi vị trí liên quan. 
3. Sắp xếp các giá trị thu thập được bên trong mỗi lớp dư lượng cho cả hai chuỗi. Việc sắp xếp được sử dụng vì thao tác cho phép sắp xếp lại tùy ý trong một lớp, do đó chỉ có nhiều tập hợp quan trọng chứ không phải thứ tự. 
4. So sánh danh sách đã sắp xếp cho từng loại dư lượng giữa$S$Và$T$. Nếu mọi lớp khớp chính xác thì việc chuyển đổi có thể thực hiện được; nếu không thì không thể được. 
5. Ghi “TAK” nếu tất cả các lớp khớp nhau và ghi “NIE” nếu ngược lại. 

### Tại sao nó hoạt động 

Hoạt động không bao giờ di chuyển một phần tử giữa các mô-đun khác nhau$k$các lớp chỉ mục, vì vậy mỗi lớp là một hệ thống con bất biến. Bên trong một lớp, các hoán đổi khối chồng chéo hoạt động giống như các hoán đổi liền kề trên một chuỗi dẫn xuất, tạo ra nhóm đối xứng đầy đủ trên lớp đó. Điều đó có nghĩa là mọi hoán vị của các giá trị bên trong lớp đều có thể truy cập được và thuộc tính duy nhất được bảo toàn là tập hợp nhiều giá trị trong mỗi lớp. Hai chuỗi tương đương chính xác khi các tập hợp này trùng khớp với mọi lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    for _ in range(n):
        parts = list(map(int, input().split()))
        lS = parts[0]
        S = parts[1:]
        
        parts = list(map(int, input().split()))
        lT = parts[0]
        T = parts[1:]
        
        k = int(input())
        
        groupsS = [[] for _ in range(k)]
        groupsT = [[] for _ in range(k)]
        
        for i, v in enumerate(S):
            groupsS[i % k].append(v)
        for i, v in enumerate(T):
            groupsT[i % k].append(v)
        
        ok = True
        for r in range(k):
            if len(groupsS[r]) != len(groupsT[r]):
                ok = False
                break
            if sorted(groupsS[r]) != sorted(groupsT[r]):
                ok = False
                break
        
        print("TAK" if ok else "NIE")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau đối số phân rã. Mỗi chuỗi được chia theo chỉ số modulo$k$, đảm bảo chúng tôi tôn trọng bất biến do hoạt động hoán đổi gây ra. Sắp xếp từng nhóm là đủ vì trong một nhóm, thứ tự hoàn toàn linh hoạt khi hoán đổi khối lặp đi lặp lại. 

Một lỗi triển khai phổ biến là quên rằng các chỉ số chứ không phải giá trị xác định nhóm. Một người khác đang cố gắng mô phỏng các giao dịch hoán đổi, điều này ngay lập tức trở nên không khả thi. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp hai chuỗi chỉ khác nhau bằng cách sắp xếp lại trong các lớp modulo. Cho phép$k = 2$,$S = [1, 2, 3, 4]$,$T = [3, 4, 1, 2]$. 

| Chỉ mục | Giá trị S | Lớp mod S | Giá trị T | Lớp mod T | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | lớp 0 | 3 | lớp 0 | 
| 1 | 2 | lớp 1 | 4 | lớp 1 | 
| 2 | 3 | lớp 0 | 1 | lớp 0 | 
| 3 | 4 | lớp 1 | 2 | lớp 1 | 

Sắp xếp lớp 0 cho$[1, 3]$trong cả hai trình tự, và lớp 1 đưa ra$[2, 4]$trong cả hai chuỗi, vì vậy câu trả lời là khẳng định. 

Bây giờ hãy xem xét$S = [1, 1, 2, 2]$,$T = [1, 2, 1, 2]$,$k = 2$. 

| Chỉ mục | Giá trị S | lớp học | Giá trị T | lớp học | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 
| 1 | 1 | 1 | 2 | 1 | 
| 2 | 2 | 0 | 1 | 0 | 
| 3 | 2 | 1 | 2 | 1 | 

Trận đấu loại 0$[1,2]$, nhưng lớp 1 thì khác: S có$[1,2]$trong khi T có$[2,2]$. Sự không khớp này cho thấy việc chuyển đổi là không thể mặc dù các tập hợp toàn cầu là tương tự nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi phần tử được đặt vào một lớp dư lượng và mỗi lớp được sắp xếp một lần | 
| Không gian |$O(n)$| Bộ nhớ để nhóm các thành phần trên tất cả các lớp | 

Tổng kích thước đầu vào trên các truy vấn được giới hạn bởi$10^5$, do đó việc phân loại theo nhóm dư lượng sẽ phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        q = int(input())
        for _ in range(q):
            parts = list(map(int, input().split()))
            lS = parts[0]
            S = parts[1:]
            
            parts = list(map(int, input().split()))
            lT = parts[0]
            T = parts[1:]
            
            k = int(input())
            
            groupsS = [[]]()
```
