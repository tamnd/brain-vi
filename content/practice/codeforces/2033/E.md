---
title: "CF 2033E - Sakurako, Kosuke và Hoán vị"
description: "Chúng ta được cho một hoán vị các số từ 1 đến n. Chúng ta được phép hoán đổi liên tục hai vị trí bất kỳ. Sau khi thực hiện một số hoán đổi, chúng tôi muốn hoán vị thỏa mãn điều kiện cấu trúc trên mọi chỉ số i."
date: "2026-06-08T11:43:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dfs-and-similar", "dsu", "graphs", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2033
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 981 (Div. 3)"
rating: 1400
weight: 2033
solve_time_s: 78
verified: true
draft: false
---

[CF 2033E - Sakurako, Kosuke và Hoán vị](https://codeforces.com/problemset/problem/2033/E) 

**Đánh giá:** 1400 
**Thẻ:** lực lượng vũ phu, cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, tham lam, toán học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ 1 đến n. Chúng ta được phép hoán đổi liên tục hai vị trí bất kỳ. Sau khi thực hiện một số hoán đổi, chúng tôi muốn hoán vị thỏa mãn điều kiện cấu trúc trên mọi chỉ số i. 

Tại mỗi vị trí i, giá trị đã nằm ở vị trí riêng của nó, nghĩa là p[i] = i hoặc nó tạo thành một cặp tương hỗ với vị trí chính xác của nó, nghĩa là nếu p[i] = x thì p[x] phải bằng i. Vì vậy, mọi phần tử đều là điểm cố định hoặc là một phần của 2 chu kỳ. Không còn chu kỳ nào được phép trong sự sắp xếp cuối cùng. 

Nhiệm vụ là tính toán số lần hoán đổi tối thiểu cần thiết để chuyển đổi hoán vị đã cho thành bất kỳ hoán vị nào thỏa mãn điều kiện này. 

Các ràng buộc rất quan trọng ở đây. Tổng n trên tất cả các trường hợp thử nghiệm lên tới 10^6, loại trừ mọi thứ bậc hai cho mỗi bài kiểm tra. Bất kỳ giải pháp nào cố gắng mô phỏng các giao dịch hoán đổi hoặc lặp lại các chu kỳ sửa chữa bằng cách quét mảng sẽ không thành công. Chúng ta cần một cách tiếp cận phân rã tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại tinh tế đối với lối suy nghĩ ngây thơ là giả định rằng mọi chu kỳ đều có thể được cố định một cách độc lập bằng một công thức cố định mà không xem xét cách thức các giao dịch hoán đổi tương tác trên toàn cầu. Ví dụ: trong 3 chu kỳ như [2, 3, 1], người ta có thể nhầm tưởng rằng nó có thể được sửa chữa trong 1 lần hoán đổi, nhưng thực tế nó cần 2 lần hoán đổi để chia nó thành các điểm cố định hợp lệ hoặc 2 chu kỳ. 

Một trường hợp tinh tế khác là sự kết hợp của các chu trình, chẳng hạn như nhiều chu trình rời rạc có độ dài khác nhau. Một bản sửa lỗi cục bộ tham lam trên mỗi chu kỳ vẫn phải tính tổng chính xác trên toàn bộ cấu trúc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng mô phỏng các giao dịch hoán đổi cho đến khi hoán vị trở nên hợp lệ. Sau mỗi lần hoán đổi, chúng tôi sẽ quét tất cả các chỉ số và kiểm tra xem mọi vị trí có thỏa mãn điều kiện hay không. Mỗi lần quét tốn O(n) và trong trường hợp xấu nhất, chúng tôi có thể cần hoán đổi O(n), dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm, quá lớn đối với n lên tới 10^6. 

Quan sát quan trọng là cấu trúc cuối cùng cực kỳ hạn chế: mọi thành phần được kết nối trong biểu đồ hoán vị phải trở thành một điểm cố định hoặc một cặp. Điều này ngay lập tức gợi ý làm việc theo chu kỳ hoán vị. 

Mỗi hoán vị có thể được phân tách thành các chu trình rời rạc. Bên trong một chu trình có độ dài k, chúng ta phải quyết định cần bao nhiêu lần hoán đổi để chuyển nó thành cấu trúc hợp lệ. Cái nhìn sâu sắc quan trọng là các giao dịch hoán đổi không trộn lẫn các chu kỳ trừ khi chúng ta cố tình phá vỡ chúng, do đó mỗi chu kỳ đóng góp độc lập cho câu trả lời. 

Đối với chu kỳ có độ dài 1, không cần gì cả. Đối với chu kỳ có độ dài 2, nó đã có giá trị như một cặp tương hỗ. Đối với bất kỳ chu trình nào có độ dài k ≥ 3, chúng ta phải chia nó thành các cặp hợp lệ và các điểm cố định bằng cách sử dụng hoán đổi. Chiến lược tối ưu là giảm từng chu kỳ như vậy bằng cách sử dụng cấu trúc có giá trị k - 1 hoán đổi, nhưng chính xác hơn, mỗi chu kỳ có độ dài k đóng góp sàn ((k - 1) / 2) hoán đổi sau khi hành vi ghép nối tối ưu xuất hiện từ phân tách hoán đổi. 

Bài toán này tương đương với việc đếm số lần hoán đổi cần thiết để chuyển đổi mỗi chu kỳ thành tập hợp 1 chu kỳ và 2 chu kỳ. Mỗi lần hoán đổi có thể khắc phục tối đa hai phần tử bị đặt sai vị trí trong một chu kỳ, điều này trực tiếp dẫn đến việc giải thích ghép nối. 

Sau khi phân tách thành các chu kỳ, chúng tôi tích lũy chi phí cho mỗi chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chu kỳ / hoán đổi vũ phu | O(n^2) | O(n) | Quá chậm | 
| Cặp đếm phân rã chu trình | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng một mảng đã truy cập và lặp qua tất cả các chỉ số từ 1 đến n. Mỗi chỉ mục chưa được truy cập sẽ bắt đầu một quá trình duyệt chu kỳ mới. 
2. Duyệt qua hoán vị bắt đầu từ chỉ mục này cho đến khi chúng ta quay lại nút đã truy cập. Thu thập kích thước của chu kỳ. Bước này tách biệt các thành phần cấu trúc độc lập, điều này là cần thiết vì các giao dịch hoán đổi trong một chu kỳ không ảnh hưởng đến chu kỳ khác. 
3. Với mỗi chu trình được phát hiện, hãy tính đóng góp của nó cho câu trả lời. Nếu độ dài chu kỳ là 1 hoặc 2, nó đóng góp 0 lần hoán đổi. Nếu độ dài chu kỳ là k ≥ 3 thì nó đóng góp sàn((k - 1) / 2). Điều này xuất phát từ việc ghép nối các phần tử một cách tối ưu trong chu kỳ bằng cách sử dụng các giao dịch hoán đổi, trong đó mỗi giao dịch hoán đổi sẽ giải quyết hai vị trí bị đặt sai vị trí theo cách sắp xếp tốt nhất có thể đạt được. 
4. Tính tổng các khoản đóng góp trong tất cả các chu kỳ và đưa ra kết quả. 

### Tại sao nó hoạt động 

Hoán vị có thể được phân tách duy nhất thành các chu trình rời rạc và hoán đổi chỉ ảnh hưởng đến các phần tử trong chu trình trừ khi được sử dụng rõ ràng để hợp nhất các chu trình. Tuy nhiên, các chu trình hợp nhất không bao giờ cải thiện được tính tối ưu vì cấu trúc đích cũng bị phân hủy thành các điểm cố định độc lập và 2 chu kỳ. Như vậy, mỗi chu trình ban đầu có thể được giải độc lập. 

Bên trong một chu trình có độ dài k, điều tốt nhất chúng ta có thể làm là chuyển đổi nó thành các điểm cố định và 2 chu kỳ hợp lệ. Mỗi lần hoán đổi có thể giảm số lượng phần tử chưa được giải quyết tối đa là 2, do đó giới hạn dưới được xác định bằng số lượng cặp chúng ta có thể tạo thành, mang lại sàn ((k - 1) / 2). Vì giới hạn này có thể đạt được bằng cách sắp xếp lại mang tính xây dựng, nên việc tính tổng các chu kỳ sẽ mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        
        visited = [False] * (n + 1)
        ans = 0

        for i in range(1, n + 1):
            if not visited[i]:
                cur = i
                cycle_len = 0
                while not visited[cur]:
                    visited[cur] = True
                    cycle_len += 1
                    cur = p[cur - 1]

                if cycle_len >= 3:
                    ans += (cycle_len - 1) // 2

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng các chu trình bằng cách sử dụng phép duyệt hoán vị tiêu chuẩn. Mỗi vị trí được truy cập chính xác một lần, đảm bảo độ phức tạp tuyến tính. Việc chuyển đổi từ chu trình sang câu trả lời sử dụng trực tiếp công thức dẫn xuất, tránh mọi mô phỏng hoán đổi. 

Chi tiết triển khai quan trọng là lập chỉ mục: hoán vị dựa trên logic 1 nhưng được lưu trữ dựa trên 0, do đó mọi chuyển đổi đều sử dụng p[cur - 1]. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
p = [2, 3, 4, 5, 1]
```| Bắt đầu | Truyền tải chu kỳ | Kích thước chu kỳ | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 → 2 → 3 → 4 → 5 → 1 | 5 | (5-1)//2 = 2 | 

Câu trả lời cuối cùng: 2 

Điều này cho thấy một chu kỳ lớn duy nhất được phân tách thành hai hoạt động hoán đổi cần thiết. 

### Ví dụ 2 

đầu vào:```
n = 4
p = [2, 3, 4, 1]
```| Bắt đầu | Truyền tải chu kỳ | Kích thước chu kỳ | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 → 2 → 3 → 4 → 1 | 4 | (4-1)//2 = 1 | 

Câu trả lời cuối cùng: 1 

Điều này xác nhận rằng các chu kỳ có độ dài chẵn không tự động hợp lệ và vẫn yêu cầu thao tác ghép nối. 

Những dấu vết này nhấn mạnh rằng cấu trúc duy nhất quan trọng là độ dài chu kỳ chứ không phải các nhãn cụ thể bên trong chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong quá trình phân tách chu trình | 
| Không gian | O(n) | Đã truy cập mảng và lưu trữ đầu vào | 

Thuật toán phù hợp thoải mái trong giới hạn vì tổng số phần tử trong các trường hợp thử nghiệm là 10^6, do đó việc truyền tải tuyến tính là tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        p = list(map(int, sys.stdin.readline().split()))
        vis = [False]*(n+1)
        ans = 0
        for i in range(1,n+1):
            if not vis[i]:
                cur = i
                cnt = 0
                while not vis[cur]:
                    vis[cur] = True
                    cnt += 1
                    cur = p[cur-1]
                if cnt >= 3:
                    ans += (cnt-1)//2
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""6
5
1 2 3 4 5
5
5 4 3 2 1
5
2 3 4 5 1
4
2 3 4 1
3
1 3 2
7
2 3 1 5 6 7 4
""") == """0
0
2
1
0
2"""

# custom cases
assert run("""1
1
1
""") == "0"

assert run("""1
3
1 2 3
""") == "0"

assert run("""1
3
2 3 1
""") == "1"

assert run("""1
6
2 1 4 3 6 5
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 danh tính | 0 | trường hợp hợp lệ nhỏ nhất | 
| hoán vị danh tính | 0 | không cần trao đổi | 
| 3 chu kỳ | 1 | chu kỳ không tầm thường tối thiểu | 
| tất cả 2 chu kỳ | 0 | cấu trúc đã hợp lệ | 

## Vỏ cạnh 

Một chu trình một phần tử như p[i] = i không đóng góp gì cả. Quá trình truyền tải ngay lập tức đánh dấu nó đã truy cập và bỏ qua đóng góp, phù hợp với yêu cầu rằng các điểm cố định đã hợp lệ. 

Chu trình hai phần tử như [2, 1] đã là một cặp tương hỗ hợp lệ. Độ dài chu kỳ là 2, do đó công thức đóng góp bằng 0 và không có giao dịch hoán đổi nào được thực hiện. 

Một chu kỳ lẻ dài chẳng hạn như [2, 3, 4, 5, 1] ​​tạo ra độ dài chu kỳ là 5. Thuật toán đóng góp 2 lần hoán đổi, tương ứng với việc sửa liên tục hai phần tử bị đặt sai vị trí trong mỗi lần hoán đổi cho đến khi chỉ còn lại các cấu trúc hợp lệ. 

Sự kết hợp rời rạc của các chu kỳ thể hiện tính độc lập. Ví dụ: [2, 1, 4, 3, 5] chia thành các chu kỳ có kích thước 2, 2 và 1, tất cả đều đóng góp bằng 0, xác nhận rằng thuật toán không trộn các thành phần không chính xác.
