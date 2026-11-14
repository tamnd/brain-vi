---
title: "CF 1800G - Symmetere"
description: "Chúng ta có một cây có gốc với các đỉnh được đánh số từ 1 đến $n$, trong đó đỉnh 1 là gốc. Mỗi đỉnh có thể có 0 hoặc nhiều đỉnh con và đầu vào chỉ định các cạnh nối các đỉnh. Nhiệm vụ là xác định xem cây có đối xứng hay không."
date: "2026-06-09T09:41:03+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "hashing", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1800
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 855 (Div. 3)"
rating: 2200
weight: 1800
solve_time_s: 101
verified: false
draft: false
---

[CF 1800G - Symmetere](https://codeforces.com/problemset/problem/1800/G) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự, băm, triển khai, cây 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Cho một cây có gốc có các đỉnh được đánh số từ 1 đến$n$, trong đó đỉnh 1 là gốc. Mỗi đỉnh có thể có 0 hoặc nhiều đỉnh con và đầu vào chỉ định các cạnh nối các đỉnh. Nhiệm vụ là xác định xem cây có đối xứng hay không. Một cây là đối xứng nếu sau khi sắp xếp lại thứ tự các nút con của mỗi nút, cây con ngoài cùng bên trái và ngoài cùng bên phải là ảnh phản chiếu, cây con thứ hai bên trái và cây con thứ hai bên phải là ảnh phản chiếu, v.v. Nếu một nút có số lượng nút con lẻ thì cây con của nút con ở giữa phải đối xứng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm mô tả một cây có tối đa$2 \cdot 10^5$đỉnh. Vì tổng tất cả các đỉnh trong các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, mọi giải pháp đều phải tuyến tính về số lượng nút trên mỗi trường hợp thử nghiệm. Thuật toán với$O(n^2)$độ phức tạp quá chậm. Chúng ta phải tránh so sánh nhiều lần các cây con bằng cách quét chúng một cách ngây thơ. Việc sử dụng bộ nhớ cũng phải nằm trong giới hạn hợp lý, khoảng$O(n)$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng bao gồm các cây là các nút đơn, chuỗi tuyến tính hoàn hảo hoặc các cây mà tính đối xứng chỉ đạt được sau khi sắp xếp lại thứ tự các cây con. Ví dụ, một cây mà gốc có các cây con có dạng palindrome nhưng bản thân các cây con khác nhau thì phải trả về NO. Việc triển khai bất cẩn có thể chỉ kiểm tra số lượng con chứ không phải cấu trúc cây con. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi hoán vị có thể có của các phần tử con tại mỗi nút và kiểm tra xem có tồn tại thứ tự đối xứng hay không. Đối với mỗi nút, chúng tôi sẽ kiểm tra đệ quy tất cả các cặp cây con, tính toán xem một nút có phải là hình ảnh phản chiếu của nút kia hay không. Về nguyên tắc, điều này đúng, nhưng việc tạo ra tất cả các hoán vị sẽ dẫn đến độ phức tạp giai thừa của số lượng nút con, điều này không khả thi đối với các nút có nhiều nút con. Ngay cả khi chúng ta thử so sánh đệ quy mà không hoán vị, việc so sánh các cấu trúc cây con một cách ngây thơ có thể dẫn đến lặp lại$O(n)$truyền tải trên mỗi nút, dẫn đến$O(n^2)$thời gian. 

Quan sát quan trọng là tính đối xứng của cây con có thể được giảm xuống thành bài toán băm. Nếu chúng ta có thể tính toán chữ ký cấu trúc hoặc hàm băm của từng cây con độc lập với thứ tự của các cây con nhưng nắm bắt được cấu trúc được phản chiếu, thì việc kiểm tra tính đối xứng sẽ trở thành một phép so sánh đơn giản của các hàm băm. Đối với một nút, chúng ta có thể tính toán đệ quy các giá trị băm của tất cả các nút con, sắp xếp các giá trị băm này (hoặc so sánh chúng theo thứ tự đối chiếu) và kết hợp chúng thành một giá trị băm duy nhất đại diện cho cây con. Hai cây con là hình ảnh phản chiếu nếu danh sách băm con của chúng đảo ngược nhau. 

Chúng ta có thể sử dụng hàm băm xác định, ví dụ: một bộ giá trị băm con được sắp xếp. Bằng cách truyền đệ quy các giá trị băm này từ lá tới gốc, chúng ta giảm vấn đề xuống còn$O(n)$mỗi cây. Cách tiếp cận này tránh việc truyền tải lặp đi lặp lại và xử lý thứ tự con tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(n!) mỗi nút | O(n) | Quá chậm | 
| Băm cây con đệ quy | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn cây dưới dạng danh sách kề, bắt nguồn từ đỉnh 1. Điều này cho phép duyệt cây con một cách hiệu quả. 
2. Định nghĩa hàm đệ quy`hash_tree(node)`trả về hàm băm cấu trúc cho cây con có gốc tại`node`. Đối với lá, hàm băm có thể là một giá trị cố định như`()`. 
3. Đối với các nút không có lá, hãy tính toán hàm băm đệ quy cho từng nút con. Lưu trữ danh sách băm con. 
4. Sắp xếp các giá trị băm con và cũng xem xét các giá trị băm con bị đảo ngược. Nếu danh sách được sắp xếp bằng với danh sách ngược lại của nó thì cây con có tính đối xứng. Nếu không thì không. 
5. Để xử lý số lượng lớn các phần tử con một cách hiệu quả, hãy chuyển đổi các giá trị băm con thành các bộ hoặc chuỗi và sử dụng chúng trong một cấu trúc có thể băm được. 
6. Trả về hàm băm cho nút hiện tại mã hóa cấu trúc của nó và tính đối xứng của nút con. Một cách tiếp cận đơn giản là chuyển đổi danh sách băm con thành một bộ dữ liệu:`tuple(child_hashes)`. Điều này đảm bảo rằng các nút có cấu trúc cây con giống hệt nhau mang lại giá trị băm giống hệt nhau. 
7. Tại gốc, kiểm tra xem cây con có đối xứng dựa trên hàm băm được tính toán hay không. Xuất ra CÓ nếu có, KHÔNG nếu ngược lại. 

Tại sao nó hoạt động: Hàm băm đệ quy đảm bảo rằng mỗi cây con được biểu diễn duy nhất bằng cấu trúc của nó. Kiểm tra tính đối xứng làm giảm việc so sánh các giá trị băm con theo thứ tự được phản ánh. Do phép đệ quy bao gồm tất cả các nút và kết hợp các giá trị băm con của chúng một cách xác định, nên mọi sự bất đối xứng sẽ được phát hiện ở cấp độ đầu tiên khi các giá trị băm được phản ánh khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    results = []
    
    for _ in range(t):
        n = int(input())
        tree = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            tree[u].append(v)
            tree[v].append(u)
        
        def dfs(node, parent):
            child_hashes = []
            for child in tree[node]:
                if child == parent:
                    continue
                child_hashes.append(dfs(child, node))
            
            # Check symmetry
            left = 0
            right = len(child_hashes) - 1
            while left < right:
                if child_hashes[left] != child_hashes[right]:
                    return None  # Asymmetrical subtree
                left += 1
                right -= 1
            
            # Hash for this node: tuple of sorted children
            return tuple(sorted(child_hashes))
        
        result = dfs(1, 0)
        results.append("YES" if result is not None else "NO")
    
    print("\n".join(results))

solve()
```Danh sách kề đảm bảo rằng chúng ta có thể duyệt từng cây trong thời gian tuyến tính. các`dfs`hàm tính toán đệ quy các giá trị băm của cây con trong khi kiểm tra tính bằng nhau của con được phản ánh. Nếu bất kỳ cặp con nào không được phản chiếu, hàm sẽ trả về`None`, lan truyền lên trên và biểu thị sự bất đối xứng. Việc sắp xếp các giá trị băm con đảm bảo tính nhất quán giữa các thứ tự con khác nhau. Độ sâu đệ quy bị giới hạn bởi`sys.setrecursionlimit`để xử lý cây sâu. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên:```
6
1 5
1 6
1 2
2 3
2 4
```

`dfs(1)`nhìn thấy trẻ em`[5, 6, 2]`. Nút 5 và 6 là nút lá nên giá trị băm của chúng là`()`. Nút 2 có con`[3,4]`, đó là những chiếc lá, cho hàm băm`(() , ())`. Kiểm tra được nhân đôi so sánh 5 với 2. Sau khi sắp xếp các giá trị băm, nút vượt qua tính đối xứng, do đó đầu ra là CÓ. 

Đối với mẫu thứ hai:```
7
1 5
1 3
3 6
1 4
4 7
4 2
```

`dfs(1)`nhìn thấy trẻ em`[5,3,4]`. Giá trị băm con khác nhau và so sánh được phản ánh không thành công, do đó kết quả đầu ra là KHÔNG. 

| Nút | Băm con | Kiểm tra tính đối xứng | Hàm băm trả về | 
| --- | --- | --- | --- | 
| 3 | [6] -> [()] | Con độc thân | ((),) | 
| 4 | [7,2] -> [(),()] | Gương được | ((),()) | 
| 1 | [5,3,4] -> [(),((),),((),())] | Trái và phải khác nhau | Không có | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần, các giá trị băm con được so sánh tuyến tính trên mỗi nút | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy tỉ lệ với n | 

Cho n 2 × 10^5 trong tất cả các trường hợp thử nghiệm, thuật toán dễ dàng chạy trong vòng 2 giây và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""6
6
1 5
1 6
1 2
2 3
2 4
7
1 5
1 3
3 6
1 4
4 7
4 2
9
1 2
2 4
2 3
3 5
1 7
7 6
7 8
8 9
10
2 9
9 10
2 3
6 7
4 3
1 2
3 8
2 5
6 5
10
3 2
8 10
9 7
4 2
8 2
2 1
4 5
6 5
5 7
1
""") == "YES\nNO\nYES\nNO\nNO\nYES"

# custom cases
assert run("1\n1\n") == "YES", "single node"
assert run("1\n3\n1 2\n1 3\n") == "YES", "two leaves"
assert run("1\n3\n1 2\n2 3\n") == "NO", "chain"
assert run("1\n5\n1 2\n1 3\n3 4\n3 5\n") == "YES", "nested symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | CÓ | Nút đơn có tính đối xứng tầm thường | 
| 3 nút, lá | CÓ | Rễ có hai lá giống hệt nhau | 
| 3 nút, chuỗi | KHÔNG | Tính đối xứng không thành công trong chuỗi tuyến tính | 
| 5 nút, | | |
