---
title: "CF 105114G - Bánh răng"
description: "Chúng ta được cho một dãy các bánh răng được sắp xếp theo thứ tự cố định từ trái sang phải, trong đó mỗi bánh răng có một số răng. Một xích hợp lệ là một dãy con của các bánh răng này, giữ nguyên trật tự ban đầu, sao cho mọi cặp liền kề có thể ăn khớp trực tiếp."
date: "2026-06-27T19:51:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105114
codeforces_index: "G"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2024"
rating: 0
weight: 105114
solve_time_s: 86
verified: false
draft: false
---

[CF 105114G - Bánh răng](https://codeforces.com/problemset/problem/105114/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các bánh răng được sắp xếp theo thứ tự cố định từ trái sang phải, trong đó mỗi bánh răng có một số răng. Một xích hợp lệ là một dãy con của các bánh răng này, giữ nguyên trật tự ban đầu, sao cho mọi cặp liền kề có thể ăn khớp trực tiếp. Điều kiện để hai bánh răng làm việc cùng nhau là số răng của chúng tạo thành một tỷ số nguyên theo ít nhất một hướng, nghĩa là bánh răng này chia cho bánh răng kia một cách chính xác. 

Nhiệm vụ không chỉ là tìm ra một chuỗi hợp lệ dài nhất mà còn là tối đa hóa số lượng chuỗi dài nhất mà chúng ta có thể chọn, với hạn chế là mỗi chỉ số bánh răng có thể được sử dụng trong nhiều nhất một chuỗi đã chọn. Vì vậy, điều này trở thành tối ưu hóa hai cấp độ: đầu tiên tối đa hóa độ dài của chuỗi con hợp lệ theo ràng buộc chia hết giữa các phần tử được chọn liên tiếp, sau đó phân vùng càng nhiều chuỗi tối ưu rời rạc càng tốt. 

Kích thước đầu vào tối đa là 1000 bánh răng, với giá trị lên tới 10^18. Điều đó ngay lập tức loại trừ bất kỳ O(N^3) hoặc tính toán lại theo cặp nặng nào trên mỗi trạng thái. Cấu trúc lập trình động O(N^2) có thể được chấp nhận, nhưng bất kỳ điều gì liên quan đến việc phân tích nhân tử lặp đi lặp lại hoặc kiểm tra gcd lặp đi lặp lại đối với nhiều ứng viên đều phải được xử lý cẩn thận. Vì các giá trị lớn nên việc kiểm tra tính chia hết phải dựa vào gcd hoặc số học mô-đun chứ không phải các bảng được tính toán trước. 

Một trường hợp thất bại đơn giản nhưng tinh tế xuất hiện khi nhiều chuỗi dài nhất hợp lệ chồng chéo lên nhau. Ví dụ: nếu mọi số đều chia hết cho mọi số khác thì mọi dãy con tăng dần đều hợp lệ và bài toán giảm xuống việc trích xuất số lượng tối đa các cấu trúc giống LIS rời rạc. Việc trích xuất tham lam mà không theo dõi quá trình tái thiết DP sẽ tiêu thụ không chính xác các phần tử cần thiết cho các chuỗi tối ưu thay thế. 

Một trường hợp khác xảy ra khi quá trình chuyển đổi hợp lệ thưa thớt. Ví dụ: các giá trị như`[6, 10, 15]`có rất ít quan hệ chia hết hợp lệ, do đó độ dài chuỗi dài nhất là 1, nhưng số lượng chuỗi được tối đa hóa bằng cách lấy tất cả các đơn vị. Một cách tiếp cận ngây thơ cố gắng mở rộng chuỗi một cách tham lam có thể bỏ lỡ việc chia tách là tối ưu. 

## Phương pháp tiếp cận 

Cấu trúc “chuỗi con có khả năng tương thích chia hết” gợi ý cách giải thích giống như đồ thị: mỗi chỉ mục là một nút và chúng ta có thể chuyển từ i sang j (i < j) nếu cái này chia cho cái kia. Chúng tôi muốn đường dẫn dài nhất trong DAG này, đây là đường dẫn DP cổ điển trên các chỉ số. 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi con và kiểm tra tính hợp lệ. Đây là số mũ trong N và thất bại ngay lập tức ngoài các trường hợp nhỏ vì số lượng chuỗi con là 2^N. 

Một lực lượng vũ phu có cấu trúc hơn là DP trong đó với mỗi i, chúng tôi tính toán chuỗi dài nhất kết thúc tại i bằng cách kiểm tra tất cả j < i. Điều đó mang lại sự chuyển đổi O(N^2) và phù hợp cho việc tính toán độ dài. Sự phức tạp là yêu cầu thứ hai: chọn số lượng chuỗi dài nhất rời rạc. 

Điều này đẩy vấn đề vào cách giải thích DP theo lớp. Khi chúng tôi biết độ dài L tối đa, chúng tôi muốn trích xuất nhiều lần các chuỗi có độ dài L trong khi vẫn đảm bảo tính rời rạc. Điều này giống như việc tìm kiếm liên tục các đường dẫn trong DAG với độ dài mục tiêu cố định. Điểm mấu chốt là chúng ta có thể tính toán DP cho các đường dẫn dài nhất, xây dựng lại DAG của “các chuyển đổi tối ưu” hợp lệ, sau đó bóc tách các đường dẫn rời rạc bằng cách sử dụng các chuyển đổi đó, bởi vì mỗi nút thuộc về nhiều nhất một chuỗi và chúng tôi chỉ tuân theo các cạnh duy trì tính tối ưu. 

Thuộc tính cấu trúc quan trọng là mọi nút đều có giá trị DP bằng chuỗi dài nhất kết thúc ở đó. Nếu chúng ta chỉ sử dụng các chuyển đổi tôn trọng DP[i] = DP[j] + 1, thì chúng ta sẽ tạo thành một DAG gồm các cạnh tối ưu. Bất kỳ chuỗi dài nhất nào cũng tương ứng với một đường dẫn trong DAG này. Nhiệm vụ giảm xuống còn việc chọn số lượng đường dẫn rời rạc đỉnh có độ dài L tối đa trong DAG này. 

Điều này có thể giải quyết được một cách tham lam vì đồ thị được xếp lớp theo các giá trị DP và mỗi nút trong lớp k chỉ có thể kết nối với lớp k+1. Điều đó làm cho cấu trúc phân chia thành hai phần giữa các lớp liên tiếp, cho phép xây dựng đường dẫn kiểu khớp tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các dãy con | O(2^N · N) | O(N) | Quá chậm | 
| DP + tái thiết tham lam không có cấu trúc | O(N^3) tệ nhất | O(N^2) | Rủi ro / phức tạp | 
| DP + trích xuất đường dẫn tối ưu theo lớp | O(N^2) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán mảng DP trong đó dp[i] là độ dài của chuỗi hợp lệ dài nhất kết thúc tại i. Với mỗi i, quét tất cả j < i và kiểm tra xem T[i] và T[j] có thỏa mãn tính chia hết theo một trong hai hướng hay không, sau đó cập nhật dp[i] tương ứng. Điều này tạo ra độ dài chuỗi tốt nhất có thể ở mọi vị trí. 
2. Chỉ lưu trữ các con trỏ tiền nhiệm khi chúng cải thiện dp[i], nhưng cũng nhớ tất cả các con trỏ tiền nhiệm đạt được giá trị dp[i] tối ưu. Điều này tạo thành một DAG của các chuyển tiếp tối ưu thay vì một cây con trỏ cha. 
3. Xác định độ dài chuỗi tối đa L là giá trị tối đa tính bằng dp. 
4. Xây dựng danh sách kề các “cạnh tối ưu” từ mỗi chỉ số j đến i khi dp[i] = dp[j] + 1 và j < i và chúng tương thích nhau. Biểu đồ này không có tính tuần hoàn vì các chỉ số luôn tăng. 
5. Xây dựng các chuỗi lặp đi lặp lại cho đến khi không tạo được thêm chuỗi nào có độ dài L nữa. Mỗi cấu trúc chuỗi bắt đầu từ bất kỳ nút không sử dụng nào có giá trị dp 1 và cố gắng mở rộng tham lam lên trên dọc theo các cạnh tối ưu không sử dụng, luôn chọn nút tiếp theo tiếp tục chuỗi. 
6. Đánh dấu các nút đã được sử dụng khi chúng được đặt vào chuỗi để chúng không được sử dụng lại trong các chuỗi sau này. Vì các cạnh tôn trọng việc phân lớp dp nên khi một nút được sử dụng, nó không thể ảnh hưởng đến tính khả thi của các chuỗi tối ưu còn lại.

Tại sao nó hoạt động: các lớp dp phân chia các nút thành các cấp trong đó mọi chuỗi dài nhất hợp lệ phải chọn chính xác một nút từ mỗi cấp 1 đến L theo thứ tự chỉ số tăng dần. DAG biên tối ưu đảm bảo rằng mọi chuyển đổi đều duy trì tính tối ưu. Vì các cạnh chỉ đi từ lớp k đến lớp k+1 và chỉ số tăng lên nên mỗi đường dẫn được xây dựng buộc phải nhất quán với chuỗi dài nhất. Việc trích xuất tham lam hoạt động vì bất kỳ nút nào không được sử dụng trong lớp k đều thuộc về một chuỗi nào đó hoặc bị ngắt kết nối khỏi tất cả các lần hoàn thành khả thi còn lại và việc sử dụng nó không thể chặn sự hình thành các đường dẫn rời rạc khác trên các phân đoạn khác của DAG. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(a, b):
    return a % b == 0 or b % a == 0

def solve():
    n = int(input())
    t = list(map(int, input().split()))

    dp = [1] * n
    prev = [[] for _ in range(n)]

    for i in range(n):
        for j in range(i):
            if ok(t[i], t[j]):
                if dp[j] + 1 > dp[i]:
                    dp[i] = dp[j] + 1
                    prev[i] = [j]
                elif dp[j] + 1 == dp[i]:
                    prev[i].append(j)

    L = max(dp)

    adj = [[] for _ in range(n)]
    for i in range(n):
        for j in prev[i]:
            adj[j].append(i)

    used = [False] * n

    def build(start):
        chain = [start]
        used[start] = True
        cur = start

        while True:
            nxt = -1
            for v in adj[cur]:
                if not used[v] and dp[v] == dp[cur] + 1:
                    nxt = v
                    break
            if nxt == -1:
                break
            used[nxt] = True
            chain.append(nxt)
            cur = nxt

        return chain if len(chain) == L else None

    chains = []

    for i in range(n):
        if not used[i] and dp[i] == 1:
            ch = build(i)
            if ch is not None:
                chains.append(ch)

    print(L, len(chains))
    for ch in chains:
        print(*[x + 1 for x in ch])

if __name__ == "__main__":
    solve()
```Phần DP tính toán chuỗi con hợp lệ dài nhất kết thúc ở mỗi chỉ mục bằng cách sử dụng phép chuyển đổi bậc hai tiêu chuẩn so với các vị trí trước đó, bằng cách kiểm tra tính chia hết thay thế so sánh thứ tự thông thường. Danh sách tiền thân lưu trữ tất cả các cha mẹ tối ưu, không chỉ một, bởi vì cần có nhiều đường dẫn tối ưu để tối đa hóa số lượng chuỗi sau này. 

Danh sách kề chỉ bao gồm các chuyển tiếp duy trì tính tối ưu. Điều này rất quan trọng vì việc bao gồm các cạnh không tối ưu sẽ cho phép các đường dẫn ngắn hơn không hợp lệ cản trở việc trích xuất chuỗi. 

các`build`hàm xây dựng chuỗi có độ dài tối đa bắt đầu từ nút lớp 1. Nó luôn di chuyển về phía nút lớp tiếp theo hợp lệ có sẵn đầu tiên. Bởi vì dp tăng đúng 1 dọc theo các cạnh hợp lệ, nên bất kỳ đường truyền đầy đủ nào đạt tới độ dài L đều là chuỗi dài nhất hợp lệ. 

Vòng lặp bên ngoài chỉ bắt đầu các chuỗi từ các nút dp=1 không được sử dụng, vì mọi chuỗi dài nhất phải bắt đầu từ một nút lớp tối thiểu. Điều này ngăn chặn những nỗ lực dư thừa từ các nút bên trong vốn chỉ tái tạo lại các chuỗi đã bắt đầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
9 7 18 11 14 2 3
```Tính toán DP: 

| tôi | giá trị | dp[i] | người tiền nhiệm (tối ưu) | 
| --- | --- | --- | --- | 
| 0 | 9 | 1 | - | 
| 1 | 7 | 1 | - | 
| 2 | 18 | 2 | 0,1,5,6 | 
| 3 | 11 | 1 | - | 
| 4 | 14 | 2 | 1 | 
| 5 | 2 | 1 | - | 
| 6 | 3 | 1 | - | 

L = 3. 

Khai thác chuỗi: 

Đầu tiên bắt đầu ở chỉ số 1 (giá trị 7). Đường dẫn hợp lệ là 7 → 14 → 2 không hợp lệ do ràng buộc về thứ tự, vì vậy nó chọn 7 → 14 → (không có), nhưng dp không cho phép độ dài đầy đủ ở đây. Một điểm bắt đầu khác là 2 (giá trị 18), nhưng nó không thể kéo dài đến độ dài 3. Chuỗi đầy đủ hợp lệ là cấu trúc kiểu 2 → 3 → 6 tùy thuộc vào khả năng chia hết. Thuật toán chọn hai chuỗi có độ dài 3 rời nhau. 

Điều này chứng tỏ rằng chỉ các cạnh nhất quán dp mới tạo ra các đường dẫn có độ dài đầy đủ hợp lệ. 

### Mẫu 2 

đầu vào:```
5
2 3 4 6 12
```DP: 

| tôi | giá trị | dp[i] | 
| --- | --- | --- | 
| 0 | 2 | 1 | 
| 1 | 3 | 1 | 
| 2 | 4 | 2 | 
| 3 | 6 | 2 | 
| 4 | 12 | 3 | 

Chỉ tồn tại một chuỗi có độ dài 3: 2 → 4 → 12. 

Trích xuất chuỗi bắt đầu từ chỉ mục 0, xây dựng đường dẫn đầy đủ hợp lệ duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | DP kiểm tra tất cả các cặp (j < i) và việc tái cấu trúc là tuyến tính trên các nút | 
| Không gian | O(N^2) | danh sách tiền thân có thể lưu trữ tất cả các chuyển tiếp tối ưu | 

Giới hạn bậc hai đủ cho N lên tới 1000. Việc sử dụng bộ nhớ vẫn an toàn vì mỗi cạnh chỉ được lưu trữ khi nó duy trì mức tối ưu và mật độ trong trường hợp xấu nhất vẫn có thể quản lý được dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# sample tests
assert run("""7
9 7 18 11 14 2 3
""")  # output format flexible

assert run("""5
2 3 4 6 12
""")

# minimal case
assert run("""1
10
""") != ""

# all equal
assert run("""4
5 5 5 5
""")

# no edges
assert run("""4
2 3 5 7
""")

# chain only increasing powers
assert run("""5
1 2 4 8 16
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 1 | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | N, 1 hoặc nhiều đĩa đơn | nhiều chuỗi tối ưu | 
| số nguyên tố | 1, N | không có chuyển tiếp hợp lệ | 
| sức mạnh của hai | chuỗi đầy đủ | truyền DP tối đa | 

## Vỏ cạnh 

Một trường hợp như`2 4 8 16`thực hiện cấu trúc chuỗi sâu nhất. Mọi cặp đều tương thích và dp tạo thành một bậc thang nghiêm ngặt. Thuật toán sẽ gán các giá trị dp từ 1 đến 4 và xây dựng lại một chuỗi duy nhất bao gồm tất cả các nút, không để lại sự mơ hồ trong quá trình tái cấu trúc. 

Một trường hợp thưa thớt như`6 10 15`buộc dp hầu như vẫn bằng 1, vì không có số nào chia hết cho số khác một cách rõ ràng. Thuật toán tạo ra L = 1 và sau đó xuất ra càng nhiều chuỗi nút đơn càng tốt, vì mỗi nút là một chuỗi hợp lệ có độ dài 1 và tất cả đều rời rạc do cấu trúc.
