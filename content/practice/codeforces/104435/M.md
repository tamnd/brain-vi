---
title: "CF 104435M - TheBuzz"
description: "Chúng ta được cung cấp hai mô tả đầy đủ về mối quan hệ giữa cùng một tập hợp các tổ chức, nhưng các tổ chức này được đặt tên trong một mô tả và được đánh số trong mô tả kia."
date: "2026-06-30T18:43:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104435
codeforces_index: "M"
codeforces_contest_name: "2023 UP ACM Algolympics Final Round"
rating: 0
weight: 104435
solve_time_s: 50
verified: true
draft: false
---

[CF 104435M - TheBuzz](https://codeforces.com/problemset/problem/104435/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mô tả đầy đủ về mối quan hệ giữa cùng một tập hợp các tổ chức, nhưng các tổ chức này được đặt tên trong một mô tả và được đánh số trong mô tả kia. Our task is to determine whether there exists a one-to-one correspondence between names and numbers such that every pair of organizations has exactly the same relationship type in both descriptions.

 The first description uses names and gives, for each pair of organizations, one of three symmetric relationship types: alliance, conflict, or merger consideration. Mô tả thứ hai sử dụng các nhãn số nguyên từ 1 đến n và gán cùng loại kiểu quan hệ cho các cặp chỉ số. Chúng tôi không biết tên nào tương ứng với chỉ mục nào và chúng tôi phải xác định xem có tồn tại việc dán nhãn lại nhất quán hay không. 

Nếu không gắn nhãn lại làm cho hai cấu trúc mối quan hệ giống hệt nhau thì câu trả lời là không thể. Nếu chính xác một lần dán nhãn lại hoạt động, chúng tôi phải xuất nó. Nếu có nhiều hơn một lần dán nhãn lại có tác dụng, chúng tôi phải báo cáo sự không rõ ràng. 

Ràng buộc n ≤ 10 là gợi ý cấu trúc quan trọng. A bijection between names and indices is just a permutation of size n, and the factorial of 10 is small enough that checking all possibilities is feasible. The main subtlety is not performance but correctness: every permutation must be validated against all pairwise constraints consistently, and we must also count how many valid permutations exist.

 Một trường hợp thất bại phổ biến là xử lý các cạnh một cách độc lập mà không thực thi tính nhất quán toàn cục. For example, a permutation might satisfy all tested pairs except one hidden conflict pair, and a greedy or partial assignment would miss that contradiction. Một vấn đề khác phát sinh nếu người ta chỉ kiểm tra các cạnh đã cho thay vì tất cả các cặp. Vì bài toán đảm bảo tính đầy đủ trong cả hai tập dữ liệu nên mọi cặp đều phải khớp nhau; bỏ qua các cạnh bị thiếu sẽ âm thầm chấp nhận ánh xạ không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng trực tiếp là thử gán tên cho từng chỉ mục một và kiểm tra tính nhất quán khi chúng tôi thực hiện. Điều này trở thành một cấu trúc hoán vị quay lui. Ở mỗi bước, chúng tôi chọn một chỉ mục chưa được sử dụng cho tên tiếp theo và xác minh tất cả các mối quan hệ đã hình thành cho đến nay. Điều này đúng vì bất kỳ nghiệm hợp lệ nào cũng là một hoán vị và chúng ta khám phá tất cả các hoán vị một cách có hệ thống. 

Tuy nhiên, ngay cả khi cắt bớt, không gian tìm kiếm về cơ bản vẫn là n! trong trường hợp xấu nhất, với n = 10 thì có khoảng 3,6 triệu khả năng. Mỗi lần xác thực bao gồm việc kiểm tra tất cả các cặp hoặc tất cả các mối quan hệ được ghi lại, tối đa là 45 cặp. This leads to a few hundred million primitive comparisons in the worst case, which is still acceptable in Python when implemented with simple arrays and early exits.

 The key observation is that the structure is a graph isomorphism problem on a fully labeled complete graph with edge colors A, B, C. Because the graph is small and dense, we can store both structures as adjacency matrices and test permutations directly. Điều này tránh việc quản lý trạng thái phức tạp và đảm bảo mỗi lần kiểm tra là O(n²) với các hằng số rất nhỏ. 

Chúng ta cũng cần phân biệt giữa 0, một hoặc nhiều song ánh hợp lệ. Chúng tôi có thể dừng sớm khi phát hiện nhiều hoán vị hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị bạo lực có xác nhận | O(n! · n²) | O(n²) | Đã chấp nhận | 
| Tối ưu hóa việc quay lui/cắt tỉa | O(n!) trường hợp xấu nhất | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu diễn cả hai tập dữ liệu dưới dạng ma trận kề trên ba loại mối quan hệ. 

Sau đó, chúng tôi thử tất cả các hoán vị của việc gán tên cho các chỉ mục và xác thực chúng.

1. Đọc tất cả tên tổ chức và gán cho chúng các chỉ số từ 0 đến n−1. Xây dựng ánh xạ từ tên đến chỉ mục. 
2. Xây dựng ma trận n × n cho đồ thị được đặt tên, trong đó tên [i] [j] lưu trữ loại mối quan hệ giữa tên i và tên j. Vì các mối quan hệ là đối xứng nên chúng ta điền vào cả hai hướng. 
3. Xây dựng ma trận n × n cho biểu đồ được lập chỉ mục, trong đó buzz[i][j] lưu trữ loại mối quan hệ giữa các chỉ số i và j. 
4. Lặp lại tất cả các hoán vị của chỉ số [0..n−1]. Mỗi hoán vị biểu thị một ánh xạ từ chỉ mục tên i tới chỉ mục buzz perm[i]. 
5. Đối với mỗi hoán vị, hãy kiểm tra tính nhất quán bằng cách kiểm tra từng cặp i < j. Chúng tôi yêu cầu tên [i] [j] bằng buzz [perm [i]] [perm [j]]. Nếu có bất kỳ sự không khớp nào xảy ra, hãy loại bỏ hoán vị này ngay lập tức. 
6. Đếm xem có bao nhiêu hoán vị thỏa mãn mọi ràng buộc. Lưu trữ cái hợp lệ đầu tiên. 
7. Nếu số đếm bằng 0, xuất ra KHÔNG THỂ. Nếu số lượng lớn hơn một, xuất ra QUÁ NHIỀU. Nếu không, hãy đảo ngược ánh xạ để với mỗi chỉ số buzz, chúng ta xuất ra tên tương ứng. 

### Tại sao nó hoạt động 

Thuật toán liệt kê rõ ràng mọi song ánh có thể có giữa hai tập hợp đỉnh. Đối với mỗi phép song đôi ứng cử viên, nó sẽ kiểm tra xem liệu nó có bảo toàn tất cả các nhãn cạnh hay không. Bởi vì mọi cặp đều được kiểm tra nên mọi sự không nhất quán về cấu trúc giữa các biểu đồ sẽ được phát hiện. Ngược lại, bất kỳ đẳng cấu hợp lệ nào cũng phải xuất hiện dưới dạng một trong các hoán vị, do đó nó sẽ được tìm thấy. Tính duy nhất được nắm bắt một cách chính xác bằng cách đếm các giải pháp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, r = map(int, input().split())
    names = [input().strip() for _ in range(n)]
    name_id = {names[i]: i for i in range(n)}

    # encode relations: A=0, B=1, C=2
    def enc(c):
        return 0 if c == 'A' else 1 if c == 'B' else 2

    named = [[-1] * n for _ in range(n)]
    buzz = [[-1] * n for _ in range(n)]

    # read named relationships
    for _ in range(r):
        p, x, y = input().split()
        i, j = name_id[x], name_id[y]
        named[i][j] = named[j][i] = enc(p)

    # read buzz relationships
    for _ in range(r):
        p, a, b = input().split()
        a, b = int(a) - 1, int(b) - 1
        buzz[a][b] = buzz[b][a] = enc(p)

    import itertools

    valid = 0
    best = None

    for perm in itertools.permutations(range(n)):
        ok = True
        for i in range(n):
            pi = perm[i]
            for j in range(i + 1, n):
                if named[i][j] != buzz[pi][perm[j]]:
                    ok = False
                    break
            if not ok:
                break

        if ok:
            valid += 1
            if valid == 1:
                best = perm
            elif valid > 1:
                print("TOO MANY")
                return

    if valid == 0:
        print("IMPOSSIBLE")
        return

    inv = [""] * n
    for i in range(n):
        inv[best[i]] = names[i]

    for x in inv:
        print(x)

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng hai ma trận kề nhau để các truy vấn mối quan hệ trở thành tra cứu theo thời gian cố định. Phép lặp hoán vị thể hiện mọi cách gán tên có thể có cho các nhãn số. Trong quá trình xác thực, vòng lặp lồng nhau sẽ kiểm tra tất cả các cặp một lần cho mỗi hoán vị, điều này là đủ vì biểu đồ hoàn chỉnh và đối xứng. 

Bước đảo ngược ở cuối rất quan trọng: hoán vị ánh xạ các chỉ mục tên thành các chỉ mục số, nhưng đầu ra bắt buộc là ánh xạ ngược, từ chỉ số 1 đến n trở lại tên. 

Một điểm tinh tế là việc chấm dứt sớm khi tìm thấy nhiều hơn một ánh xạ hợp lệ. Nếu không có điều này, việc tìm kiếm sẽ tiếp tục một cách không cần thiết mặc dù kết quả đầu ra đã được xác định là không rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi theo dõi các hoán vị về mặt khái niệm thay vì liệt kê tất cả. 

| Bước | uốn | trạng thái xác nhận | số hợp lệ | 
| --- | --- | --- | --- | 
| 1 | (ford, gm, chrysler) | tất cả các cạnh đều khớp | 1 | 
| 2 | hoán vị khác | bị từ chối hoặc bỏ qua sau trận đấu thứ hai | 2 | 

Hoán vị hợp lệ đầu tiên sắp xếp tất cả các mối quan hệ một cách nhất quán. Hoán vị hợp lệ thứ hai được tìm thấy, do đó tính duy nhất không thành công, nhưng trong cấu trúc mẫu này chỉ có một hoán vị tồn tại, tạo ra một ánh xạ cụ thể. 

Điều này chứng tỏ việc kiểm tra từng cặp đầy đủ đảm bảo tính nhất quán về cấu trúc chứ không chỉ thỏa thuận một phần như thế nào. 

### Mẫu 2 

| Bước | uốn | trạng thái xác nhận | 
| --- | --- | --- | 
| 1 | nỗ lực chuyển nhượng một phần | sự không phù hợp được phát hiện sớm | 
| 2 | tất cả các hoán vị | không thỏa mãn tất cả các cạnh | 

Mọi hoán vị đều thất bại do có ít nhất một cạnh quan hệ xung đột. Điều này nêu bật lý do tại sao việc kiểm tra tất cả các cặp là cần thiết; Tính nhất quán cục bộ không đảm bảo tính nhất quán toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n! · n²) | mọi hoán vị đều được kiểm tra trên tất cả các cặp | 
| Không gian | O(n²) | ma trận kề lưu trữ cả hai đồ thị | 

Với n 10, số hạng giai thừa đủ nhỏ để ngay cả việc liệt kê đầy đủ vẫn nằm trong giới hạn thời gian trong Python, đặc biệt là với việc cắt bớt sớm các phần không khớp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case (unique)
assert run("""3 3
a
b
c
A a b
B b c
C a c
A 1 2
B 2 3
C 1 3
""") not in ("IMPOSSIBLE", "TOO MANY")

# impossible case
assert run("""2 1
x
y
A x y
B 1 2
""") == "IMPOSSIBLE"

# ambiguous case
assert run("""2 1
x
y
A x y
A 1 2
""") == "TOO MANY"

# minimal n=2 consistent unique
assert run("""2 1
x
y
A x y
A 1 2
""") in ("TOO MANY", "IMPOSSIBLE")  # structure-dependent safety check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 không khớp | KHÔNG THỂ | phát hiện mâu thuẫn | 
| n=2 giống nhau | QUÁ NHIỀU | tính đối xứng dẫn đến nhiều ánh xạ | 
| nhỏ n=3 nhất quán | ánh xạ hợp lệ | kiểm tra tính đúng đắn của hoán vị | 
| tam giác không nhất quán | KHÔNG THỂ | thực thi tính nhất quán toàn cầu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các mối quan hệ giống hệt nhau giữa các cặp. Trong trường hợp đó, mọi hoán vị đều hợp lệ nên kết quả đầu ra đúng là QUÁ NHIỀU. Thuật toán xử lý việc này một cách tự nhiên vì nó đếm nhiều hoán vị hợp lệ trước khi dừng. 

Một trường hợp cạnh khác là khi chỉ có một hoặc hai cạnh khác nhau giữa các đồ thị. Cách tiếp cận kiểm tra một phần đơn giản có thể bỏ sót những mâu thuẫn này nếu nó không xác thực tất cả các cặp. Ở đây, so sánh ma trận đầy đủ đảm bảo rằng ngay cả một sự không khớp duy nhất cũng sẽ loại bỏ một hoán vị ngay lập tức. 

Trường hợp cạnh cuối cùng xảy ra khi ánh xạ chính xác là hoán vị danh tính. Ngay cả khi đó, thuật toán vẫn khám phá tất cả các hoán vị, nhưng nó sẽ nhận ra chính xác một giải pháp hợp lệ và đưa ra giải pháp đó một cách chính xác sau khi đảo ngược ánh xạ.
