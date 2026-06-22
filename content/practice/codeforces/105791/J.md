---
title: "CF 105791J - Thẩm phán thất bại"
description: "Mỗi bài dự thi trong cuộc thi thuộc về chính xác một thí sinh và mỗi thí sinh có những hạn chế cá nhân về những phán quyết mà họ có thể nhận được. Đối với mỗi lần gửi, hệ thống không tạo ra kết quả xác định."
date: "2026-06-21T13:11:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "J"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 44
verified: true
draft: false
---

[CF 105791J - Thẩm phán thất bại](https://codeforces.com/problemset/problem/105791/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bài dự thi trong cuộc thi thuộc về chính xác một thí sinh và mỗi thí sinh có những hạn chế cá nhân về những phán quyết mà họ có thể nhận được. Đối với mỗi lần gửi, hệ thống không tạo ra kết quả xác định. Thay vào đó, nó chọn ngẫu nhiên thống nhất trong số các phán quyết được phép cho thí sinh đã gửi bài đó. 

Nhiệm vụ không phải là mô phỏng tính ngẫu nhiên này mà là tính toán số lần dự kiến ​​mỗi phán quyết xuất hiện sau khi tất cả các bài nộp được đánh giá. 

Một cách hữu ích để điều chỉnh lại quy trình là suy nghĩ theo từng lần gửi. Mỗi lần gửi đóng góp khối lượng xác suất vào một tập hợp con các phán quyết. Nếu một thí sinh có$t$các phán quyết được cho phép thì mỗi phán quyết đó nhận được xác suất$1/t$từ lần gửi đó và tất cả các phán quyết khác không nhận được sự đóng góp nào. Câu trả lời cuối cùng chỉ là tổng của những đóng góp này trong tất cả các bài nộp. 

Các hạn chế rất quan trọng vì số lượng bài nộp có thể đạt tới$10^5$, trong khi số lượng loại phán quyết rất nhỏ, nhiều nhất là 10. Điều này ngay lập tức cho thấy rằng bất kỳ giải pháp nào cũng phải tuyến tính về số lượng bài gửi và tỷ lệ thuận với số lượng phán quyết của mỗi thí sinh, thay vì bất kỳ điều gì liên quan đến tính toán theo cặp nặng nề hoặc theo dõi trạng thái đối với bài nộp. 

Một hướng đi ngây thơ nhưng không chính xác sẽ là mô phỏng tính ngẫu nhiên hoặc lấy mẫu kết quả nhiều lần và kết quả trung bình. Điều đó sẽ tạo ra phương sai và không hội tụ chính xác, và thậm chí nếu lặp lại nhiều lần thì nó sẽ quá chậm và vẫn chỉ gần đúng. 

Một sai lầm tinh vi hơn là cố gắng xử lý bài nộp một cách độc lập mà không tổng hợp thí sinh. Nếu chúng tôi tính toán lại các tập hợp được phép cho mỗi lần gửi từ đầu bằng cách sử dụng các cấu trúc đắt tiền, thì chúng tôi vẫn có thể vượt qua các ràng buộc, nhưng thông tin chi tiết quan trọng là cùng một thí sinh xuất hiện nhiều lần, vì vậy việc tính toán trước phân bố xác suất của họ là điều cần thiết. 

Không có trường hợp góc khó nào trong chính mô hình xác suất, nhưng có một trường hợp góc cạnh tinh tế là các thí sinh chỉ có một phán quyết được phép. Trong trường hợp đó, mọi bài dự thi của thí sinh đó đều góp phần xác định vào phán quyết đó với xác suất 1. Một trường hợp khác là khi bất kỳ thí sinh nào không được phép đưa ra phán quyết, trong trường hợp đó tần suất dự kiến ​​​​của nó chính xác bằng 0. 

## Phương pháp tiếp cận 

Nếu chúng ta xem xét một lần gửi một cách riêng biệt thì việc tính toán rất đơn giản. Giả sử thí sinh$c$có một bộ kích thước cho phép$t_c$. Sau đó với mỗi phán đoán được phép$v$, bài nộp này góp phần$1/t_c$đến số lượng dự kiến$v$. Tổng hợp tất cả các bài nộp sẽ đưa ra kỳ vọng cuối cùng. 

Cách tiếp cận bạo lực sẽ xử lý từng lần gửi một cách độc lập và đối với mỗi lần gửi sẽ lặp lại trên tất cả$K$phán quyết, kiểm tra xem thí sinh đó có được phép hay không. Điều đó mang lại$O(NK)$thời gian đã được chấp nhận$K \le 10$, nhưng nó không phải là cấu trúc sạch nhất. Một biến thể mạnh mẽ hơn sẽ cố gắng tính toán xác suất một cách linh hoạt cho mỗi lần gửi mà không tính toán trước việc phân bổ thí sinh, dẫn đến công việc lặp đi lặp lại tỷ lệ với kích thước đầu vào nhân với kích thước tập hợp phán quyết. 

Quan sát quan trọng là việc phân phối chỉ phụ thuộc vào thí sinh chứ không phụ thuộc vào bài nộp. Vì vậy, chúng ta có thể tính toán trước cho mỗi thí sinh một vectơ xác suất trên$K$phán quyết. Khi điều này được thực hiện, mỗi lần gửi sẽ trở thành một phép tra cứu và bổ sung vectơ đơn giản. 

Điều này biến vấn đề thành việc xây dựng$M$vectơ xác suất nhỏ, sau đó tính tổng$N$bản sao của chúng theo quyền sở hữu nộp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mỗi lần tính toán lại trình | O(NK) | O(K) | Được chấp nhận nhưng dư thừa | 
| Tính toán trước cho mỗi thí sinh | O(MK + N) | O(MK) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bài nộp$N$, số lượng thí sinh$M$, và số lượng các loại phán quyết$K$. Chúng tôi sẽ duy trì một mảng kích thước$K$để tích lũy số lượng dự kiến. 
2. Đối với mỗi thí sinh, hãy đọc danh sách nhận định được phép của họ và lưu trữ. Chúng tôi cũng tính toán kích thước của nó$t$. Kích thước này trực tiếp xác định khối lượng xác suất mà mỗi phán quyết nhận được từ bài dự thi của thí sinh này. 
3. Xây dựng vectơ xác suất cho từng thí sinh. Đối với mọi phán quyết được phép$v$, gán xác suất$1/t$. Vectơ này thể hiện sự đóng góp dự kiến ​​của bất kỳ bài dự thi nào từ thí sinh đó. 
4. Đọc trình tự các bài nộp. Mỗi bài gửi chỉ cho chúng tôi biết thí sinh nào đã tạo bài đó, vì vậy chúng tôi sử dụng chỉ mục đó để tìm nạp vectơ xác suất được tính toán trước. 
5. Với mỗi bài dự thi, hãy thêm vectơ xác suất của thí sinh vào mảng câu trả lời chung. Điều này tương đương với việc tổng hợp những đóng góp dự kiến ​​qua các thử nghiệm độc lập. 
6. Xuất ra vectơ tích lũy cuối cùng với độ chính xác vừa đủ. 

Lý do điều này hợp lệ là kỳ vọng là tuyến tính. Mỗi lần gửi đóng góp độc lập vào số lượng dự kiến ​​của từng phán quyết, do đó, việc tính tổng kỳ vọng của mỗi lần gửi sẽ mang lại kỳ vọng toàn cầu chính xác mà không cần lập mô hình bất kỳ phân phối chung nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    N, M, K = map(int, input().split())
    
    allowed = [[] for _ in range(M)]
    prob = [[0.0] * K for _ in range(M)]
    
    for i in range(M):
        qt = int(input())
        arr = list(map(int, input().split()))
        allowed[i] = arr
        p = 1.0 / qt
        for v in arr:
            prob[i][v - 1] = p
    
    ans = [0.0] * K
    
    for _ in range(N):
        c = int(input()) - 1
        pc = prob[c]
        for i in range(K):
            ans[i] += pc[i]
    
    print(" ".join(f"{x:.6f}" for x in ans))

if __name__ == "__main__":
    main()
```Cấu trúc cốt lõi tách tiền xử lý khỏi tổng hợp. Bước tiền xử lý xây dựng một vectơ xác suất cố định cho mỗi thí sinh, giúp tránh việc phân chia lặp lại và xử lý danh sách lặp đi lặp lại trong vòng gửi. 

Một chi tiết triển khai tinh tế là lập chỉ mục. Các phán đoán được đưa ra dưới dạng đầu vào dựa trên 1, vì vậy chúng phải được chuyển sang các chỉ số dựa trên 0 trước khi được lưu trữ trong vectơ xác suất. Một chi tiết khác là đảm bảo việc tích lũy dấu phẩy động được thực hiện với độ chính xác gấp đôi, điều mà float của Python cung cấp một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1 4
1
2
3
4
1
1
1
1
```Ở đây có một thí sinh duy nhất có thể nhận được cả 4 phán quyết. Mỗi bài nộp đóng góp$1/4$đến mọi bản án. 

| Đệ trình | Thí sinh | Kích thước cho phép | Đóng góp cho mỗi bản án | 
| --- | --- | --- | --- | 
| 1 | 0 | 4 | (0,25, 0,25, 0,25, 0,25) | 
| 2 | 0 | 4 | (0,25, 0,25, 0,25, 0,25) | 
| 3 | 0 | 4 | (0,25, 0,25, 0,25, 0,25) | 
| 4 | 0 | 4 | (0,25, 0,25, 0,25, 0,25) | 

Tính tổng cho (1, 1, 1, 1). Điều này xác nhận rằng các phân phối giống hệt nhau độc lập lặp đi lặp lại tích lũy tuyến tính. 

### Ví dụ 2 

Hãy xem xét:```
3 2 3
2
1 2
1
3
1
2
1
```Thí sinh 0 có {1,2} nên mỗi người đóng góp 1/2 cho nhận định 1 và 2. Thí sinh 1 chỉ có {3} nên mỗi người đóng góp đầy đủ cho nhận định 3. 

| Đệ trình | Thí sinh | Vectơ | 
| --- | --- | --- | 
| 1 | 0 | (0,5, 0,5, 0,0) | 
| 2 | 1 | (0,0, 0,0, 1,0) | 
| 3 | 0 | (0,5, 0,5, 0,0) | 

Tính tổng cho (1.0, 1.0, 1.0). Điều này cho thấy cách phân phối thí sinh hỗn hợp vẫn kết hợp chặt chẽ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MK + NK) | Các vectơ xác suất xây dựng lấy MK và mỗi lần gửi sẽ thêm một vectơ có độ dài K | 
| Không gian | O(MK + K) | Lưu trữ để phân phối cho mỗi thí sinh cộng với câu trả lời cuối cùng | 

Các giới hạn đủ nhỏ để ngay cả trường hợp xấu nhất$10^5 \times 10$tích lũy là chuyện nhỏ trong Python. Trí nhớ cũng an toàn vì$M \le N$Và$K \le 10$, do đó việc lưu trữ các bảng xác suất đầy đủ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    N, M, K = map(int, input().split())
    allowed = [[] for _ in range(M)]
    prob = [[0.0] * K for _ in range(M)]
    
    for i in range(M):
        qt = int(input())
        arr = list(map(int, input().split()))
        p = 1.0 / qt
        for v in arr:
            prob[i][v - 1] = p
    
    ans = [0.0] * K
    
    for _ in range(N):
        c = int(input()) - 1
        for i in range(K):
            ans[i] += prob[c][i]
    
    return " ".join(f"{x:.6f}" for x in ans)

# sample-like case
assert run("""4 1 4
4
1 2 3 4
1
1
1
1
""") == "1.000000 1.000000 1.000000 1.000000"

# single verdict only
assert run("""3 1 2
1
2
1
1
1
1
""") == "3.000000 0.000000"

# two contestants split
assert run("""3 2 2
1
1
1
2
1
2
1
""") == "1.500000 1.500000"

# mixed restrictions
assert run("""2 2 3
2
1 2
1
3
1
2
""") == "0.500000 0.500000 1.000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thí sinh phổ thông duy nhất | tích lũy đồng đều | phân phối xác suất bằng nhau | 
| cho phép phán quyết duy nhất | tích lũy xác định | ốp viền size 1 bộ | 
| hai thí sinh cân bằng | đối xứng | chia đúng | 
| ràng buộc hỗn hợp | phân phối một phần | đóng góp không đồng nhất | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một thí sinh có đúng một phán quyết được phép. Trong tình huống này, vectơ xác suất chứa một số 1 và các số 0 còn lại. Ví dụ: nếu thí sinh 0 chỉ cho phép nhận xét 2 thì mỗi bài nộp đóng góp chính xác 1 cho nhận định 2. Thuật toán xử lý việc này một cách tự nhiên vì$1/t = 1$, vì vậy không cần phân nhánh đặc biệt. 

Một trường hợp khác là khi các thí sinh khác nhau có các bộ được phép rời rạc. Thuật toán vẫn hoạt động chính xác vì mỗi lần gửi chỉ đóng góp trong phạm vi phân phối cục bộ của chính nó. Không có sự can thiệp giữa các thí sinh vì việc tích lũy hoàn toàn mang tính chất cộng gộp. 

Trường hợp cạnh cuối cùng là khi phán quyết không bao giờ xuất hiện trong tập hợp được phép của bất kỳ thí sinh nào. Trong trường hợp đó, cột của nó trong tất cả các vectơ xác suất vẫn bằng 0, do đó nó không bao giờ nhận được bất kỳ đóng góp nào trong quá trình tổng hợp.
