---
title: "CF 104760I - \u041d\u0430\u043b\u043e\u0433\u043e\u0432\u044b\u0439 \u0432\u043e\u043f\u0440\u043e\u0441"
description: "Chúng ta được cung cấp một biểu đồ các hành tinh trong đó mỗi hành tinh được kết nối với một số hành tinh khác bằng các tuyến đường trực tiếp hai chiều. Đối với mỗi hành tinh, chúng ta chỉ quan tâm đến việc có bao nhiêu đường đi trực tiếp liên quan đến nó, đó là mức độ của nó theo đồ thị."
date: "2026-06-29T02:22:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "I"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 61
verified: true
draft: false
---

[CF 104760I - \u041d\u0430\u043b\u043e\u0433\u043e\u0432\u044b\u0439 \u0432\u043e\u043f\u0440\u043e\u0441](https://codeforces.com/problemset/problem/104760/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ các hành tinh trong đó mỗi hành tinh được kết nối với một số hành tinh khác bằng các tuyến đường trực tiếp hai chiều. Đối với mỗi hành tinh, chúng ta chỉ quan tâm đến việc có bao nhiêu đường đi trực tiếp liên quan đến nó, đó là mức độ của nó theo đồ thị. 

Nhiệm vụ là đếm xem có bao nhiêu hành tinh có độ nằm trong một khoảng cho trước từ P đến Q. 

Vì vậy, đầu vào mô tả một đồ thị vô hướng có N đỉnh và M cạnh. Mỗi cạnh tăng mức độ của cả hai điểm cuối lên một. Sau khi xây dựng hoặc xử lý biểu đồ này, chúng tôi tính toán bậc của mỗi đỉnh và đếm xem có bao nhiêu đỉnh nằm trong phạm vi yêu cầu. 

Các ràng buộc cho phép tối đa 10^4 hành tinh và tối đa 10^5 kết nối. Thang đo này ngụ ý rằng giải pháp O(N + M) dễ dàng đủ nhanh. Bất cứ điều gì tính toán lại thông tin kề trên mỗi nút theo cách lồng nhau, chẳng hạn như quét tất cả các cạnh cho mọi đỉnh, dẫn đến khoảng 10^9 thao tác trong trường hợp xấu nhất và sẽ không chạy trong giới hạn thời gian. 

Một trường hợp phức tạp xuất hiện khi một hành tinh không có kết nối nào cả. Bậc của nó bằng 0, vì vậy nếu P bằng 0 hoặc khoảng bao gồm 0 thì các nút bị cô lập phải được tính. Một trường hợp cạnh khác là khi M bằng 0 và N lớn, làm cho tất cả các độ bằng 0 cùng một lúc. Ngoài ra, P và Q có thể bằng 1 hoặc nhiều hơn, vì vậy các đồ thị trong đó mọi nút có độ 0 sẽ tạo ra kết quả bằng 0 một cách chính xác trừ khi P bằng 0. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải bài toán là tính bậc của mỗi đỉnh bằng cách quét tất cả các cạnh. Đối với mỗi nút, chúng ta có thể lặp qua tất cả các cạnh và đếm số lần nó xuất hiện dưới dạng điểm cuối. Điều này tạo ra mức độ chính xác vì mỗi lần xuất hiện tương ứng với một cạnh tới. 

Tuy nhiên, phương pháp này lặp lại quá trình quét tương tự cho mọi đỉnh. Với N đỉnh và M cạnh, điều này dẫn đến công việc O(NM), trong trường hợp xấu nhất đạt tới khoảng 10^9 thao tác. Như vậy là quá chậm. 

Quan sát quan trọng là việc tính toán mức độ không yêu cầu quét theo từng đỉnh. Mỗi cạnh đóng góp chính xác một đơn vị cho hai đỉnh. Vì vậy, chúng ta có thể duy trì một mảng độ và cập nhật nó một lần trên mỗi cạnh. Điều này làm giảm vấn đề xuống còn một lần vượt qua danh sách cạnh. 

Sau khi tính toán độ, chúng ta chỉ cần đếm xem có bao nhiêu nằm trong [P, Q]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét các cạnh trên mỗi nút) | O(NM) | O(N) | Quá chậm | 
| Tích lũy bằng cấp | O(N + M) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán độ tăng dần trong khi đọc các cạnh. 

1. Tạo một mảng có kích thước N được khởi tạo bằng 0. Điều này sẽ lưu trữ bao nhiêu cạnh chạm vào mỗi hành tinh. 
2. Đọc từng cạnh (a, b). Vì đồ thị là vô hướng nên hãy tăng độ[a] lên một và độ[b] lên một. Điều này trực tiếp mã hóa định nghĩa về mức độ mà không cần lưu trữ kề. 
3. Sau khi xử lý tất cả các cạnh, lặp lại trên tất cả các hành tinh từ 1 đến N. 
4. Đối với mỗi hành tinh, hãy kiểm tra xem độ[i] có nằm giữa P và Q hay không. Nếu có, hãy tăng bộ đếm câu trả lời. 
5. Xuất bộ đếm cuối cùng. 

Ý tưởng chính là mỗi cạnh được xử lý chính xác một lần và đóng góp của nó được phân phối ngay lập tức cho cả hai điểm cuối. 

### Tại sao nó hoạt động 

Bậc của một đỉnh trong đồ thị vô hướng được định nghĩa là số cạnh liên quan. Mỗi cạnh đầu vào đóng góp chính xác một tỷ lệ cho mỗi điểm cuối của nó và không có đỉnh nào khác. Bằng cách thêm 1 vào cả hai điểm cuối trong quá trình xử lý đầu vào, chúng tôi tích lũy chính xác định nghĩa về mức độ. Vì không có cạnh nào bị bỏ sót hoặc được tính hai lần ngoài hai điểm cuối của nó nên mảng kết quả là đúng cho tất cả các đỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    deg = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        deg[a] += 1
        deg[b] += 1

    p, q = map(int, input().split())

    ans = 0
    for i in range(1, n + 1):
        if p <= deg[i] <= q:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng một mảng độ đơn giản được lập chỉ mục theo id hành tinh. Chi tiết triển khai quan trọng nhất là chúng tôi hoàn toàn không xây dựng danh sách kề vì chỉ có mức độ mới quan trọng. Điều này tránh được việc tốn thêm bộ nhớ và giữ cho giải pháp tuyến tính. 

Kiểm tra giới hạn`p <= deg[i] <= q`phải bao gồm cả hai điểm cuối. Ở đây thường xảy ra những sai sót ngẫu nhiên, đặc biệt là khi diễn giải xem khoảng đó có bao gồm hay không. Vì bài toán nói rõ ràng là bao hàm nên cả hai phép so sánh đều phải không nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
1 2
1 2
```| Bước | Cạnh | độ[1] | độ[2] | 
| --- | --- | --- | --- | 
| Ban đầu | - | 0 | 0 | 
| Sau cạnh | (1,2) | 1 | 1 | 

P = 1, Q = 2 

Cả hai nút đều thỏa mãn điều kiện. 

Đầu ra là 2. 

Điều này xác nhận rằng ngay cả trong biểu đồ được kết nối nhỏ nhất, cả hai điểm cuối đều được tính đối xứng. 

### Mẫu 2 

đầu vào:```
4 3
1 2
1 3
1 4
2 3
```| Bước | Cạnh | độ[1] | độ[2] | độ[3] | độ[4] | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | 0 | 0 | 0 | 0 | 
| (1,2) | 1 | 1 | 1 | 0 | 0 | 
| (1,3) | 2 | 2 | 1 | 1 | 0 | 
| (1,4) | 3 | 3 | 1 | 1 | 1 | 

P = 2, Q = 3 

Chỉ có nút 1 có bậc 3 nên chỉ tính nút đó. 

Đầu ra là 1. 

Điều này cho thấy các trung tâm xuất hiện một cách tự nhiên như thế nào trong quá trình tích lũy mức độ và cách bộ lọc phạm vi tách biệt chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi cạnh được xử lý một lần, sau đó mỗi nút được kiểm tra một lần | 
| Không gian | O(N) | Mảng độ có kích thước N | 

Giới hạn đầu vào lên tới 10^4 nút và 10^5 cạnh giúp việc này trở nên hiệu quả một cách thoải mái. Giải pháp thực hiện theo thứ tự 10^5 thao tác, điều này không đáng kể đối với các giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    deg = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        deg[a] += 1
        deg[b] += 1

    p, q = map(int, input().split())

    ans = 0
    for i in range(1, n + 1):
        if p <= deg[i] <= q:
            ans += 1

    return str(ans)

# provided samples
assert run("""2 1
1 2
1 2
""") == "2"

assert run("""4 3
1 2
1 3
1 4
2 3
""") == "1"

# custom cases
assert run("""1 0
1 1
""") == "0", "single isolated node outside range"

assert run("""1 0
0 0
""") == "1", "single node with degree zero inside range"

assert run("""5 0
0 0
""") == "5", "all isolated nodes counted"

assert run("""3 3
1 2
2 3
1 3
2 2
""") == "1", "triangle graph only middle node excluded? boundary check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn, không có cạnh, P=1 | 0 | loại trừ độ 0 | 
| nút đơn, P=0 | 1 | bao gồm các nút độ 0 | 
| đồ thị trống có P=0 | N | tất cả các nút được tính | 
| đồ thị tam giác | 1 | tích lũy và lọc mức độ chính xác | 

## Vỏ cạnh 

Khi không có cạnh, mọi hành tinh đều có độ 0. Thuật toán vẫn khởi tạo tất cả các độ về 0 và không thực hiện cập nhật nào. Vòng lặp cuối cùng đếm xem có bao nhiêu nút thỏa mãn P ∼ 0 ∼ Q, điều này đúng vì các nút bị cô lập chỉ là ứng cử viên hợp lệ nếu số 0 nằm trong khoảng. 

Đối với biểu đồ nút đơn, vòng lặp qua các cạnh bị bỏ qua hoàn toàn. Bậc vẫn bằng 0 và câu trả lời cuối cùng phụ thuộc hoàn toàn vào việc khoảng đó có bao gồm 0 hay không. Điều này xác nhận rằng lời giải không giả sử tồn tại ít nhất một cạnh. 

Khi tất cả các nút được kết nối ở mức độ cao, chẳng hạn như một biểu đồ hoàn chỉnh, mỗi nút sẽ tích lũy độ N−1 thông qua các cập nhật đối xứng lặp đi lặp lại. Vì mỗi cạnh đóng góp chính xác hai phần tăng lên nên không xảy ra hiện tượng đếm quá mức và quá trình lọc vẫn chính xác ngay cả ở mật độ tối đa.
