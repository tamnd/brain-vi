---
title: "CF 105182C-Thêm"
description: "Chúng ta bắt đầu với một mảng có vị trí thứ i ban đầu chứa i. Sau đó, chúng tôi thực hiện một chuỗi n − 1 cập nhật ngẫu nhiên."
date: "2026-06-27T05:11:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105182
codeforces_index: "C"
codeforces_contest_name: "The 22nd UESTC Programming Contest - Final"
rating: 0
weight: 105182
solve_time_s: 55
verified: true
draft: false
---

[CF 105182C - Thêm](https://codeforces.com/problemset/problem/105182/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng có vị trí thứ i ban đầu chứa i. Sau đó, chúng tôi thực hiện một chuỗi n − 1 cập nhật ngẫu nhiên. Ở lần cập nhật thứ i, chúng tôi xem xét tiền tố thu hẹp của mảng, lấy một phần tử cụ thể từ đầu bên phải của phạm vi hoạt động hiện tại và thêm gấp đôi giá trị của nó vào một vị trí ngẫu nhiên thống nhất giữa các chỉ mục trước đó. 

Cụ thể, ở bước i, phần tử ở vị trí n − i + 1 được sử dụng làm “nguồn”. Chúng tôi chọn chỉ số j thống nhất từ ​​1 đến n − i và cộng 2 lần giá trị hiện tại của nguồn vào a[j]. Sau đó, vị trí nguồn không bao giờ được sử dụng làm mục tiêu nữa, nhưng các vị trí trước đó vẫn có thể tham gia vào các hoạt động trong tương lai với tư cách là nguồn. 

Đại lượng chúng ta được yêu cầu là giá trị của a1 sau tất cả các phép toán ngẫu nhiên này, lấy modulo 998244353. Bởi vì mỗi bước đều liên quan đến tính ngẫu nhiên nên đây là một biến ngẫu nhiên. Cách diễn giải dự định là giá trị kỳ vọng của nó, vì kết quả cuối cùng chỉ mang tính quyết định sau khi lấy kỳ vọng đối với tất cả các lựa chọn của j. 

Ràng buộc n 10^9 với tối đa 10^4 trường hợp thử nghiệm ngay lập tức loại trừ mọi mô phỏng hoặc thậm chí DP tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cũng phải thu gọn toàn bộ quá trình thành một biểu thức dạng đóng chỉ phụ thuộc vào n. 

Một vấn đề tế nhị là các giá trị được truyền đi nhiều lần. Một phần tử được thêm vào sớm sau đó có thể được chọn làm nguồn và sau đó giá trị tích lũy của nó sẽ được truyền lại. Điều này làm cho “tích lũy một lần” ngây thơ không chính xác trừ khi chúng tôi tính toán chính xác sự khuếch đại lặp đi lặp lại trong suốt quá trình. 

Một kiểm tra nhỏ về độ tỉnh táo cho thấy tại sao tính ngẫu nhiên lại quan trọng. Với n = 2, thao tác duy nhất là bắt buộc và a1 trở thành 1 + 2·2 = 5. Mọi cách diễn giải đúng đều phải phù hợp với kết quả xác định này. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ thực hiện rõ ràng từng thao tác, chọn ngẫu nhiên j và cập nhật mảng. Về nguyên tắc, điều này đúng nhưng chi phí là O(n) cho mỗi trường hợp thử nghiệm, điều này là không thể đối với n lên tới 10^9. 

Ngay cả một DP được cải tiến để theo dõi các giá trị mong đợi trên mỗi chỉ mục cũng dẫn đến sự lặp lại trong đó mỗi trạng thái phụ thuộc vào tất cả các trạng thái sau đó. Điều đó mang lại O(n^2) hoặc O(n) cho mỗi trường hợp thử nghiệm, cả hai đều hoàn toàn không khả thi. 

Quan sát quan trọng là hệ thống có giá trị tuyến tính nên chúng ta có thể phân tích các đóng góp một cách độc lập. Mỗi lần một giá trị được truyền qua một thao tác, nó sẽ được nhân với 2. Điều này gợi ý nên hiểu quy trình là một cấu trúc có gốc ngẫu nhiên trong đó mỗi nút cuối cùng đóng góp vào a1 với trọng số tùy thuộc vào số lần nó được “đẩy lên” thông qua các lựa chọn ngẫu nhiên. 

Điều này chuyển vấn đề thành hiểu sự khuếch đại dự kiến ​​dọc theo chuỗi cha mẹ ngẫu nhiên trong cây đệ quy ngẫu nhiên. Sau khi được viết lại ở dạng đó, kỳ vọng sẽ trở nên dễ điều khiển vì cấu trúc của cây đệ quy ngẫu nhiên có tính đối xứng mạnh làm giảm độ phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trực tiếp | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| DP ngây thơ qua các tiểu bang | O(n^2) | O(n) | Quá chậm | 
| Phân tích kỳ vọng cây | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quá trình này như việc xây dựng một cây đệ quy ngẫu nhiên trên các nút từ 1 đến n. Mỗi nút i chọn nút cha một cách thống nhất từ ​​nút 1 đến nút i − 1. Điều này xuất phát trực tiếp từ thực tế là tại thời điểm nút i đóng vai trò là nguồn, nó chọn mục tiêu j một cách thống nhất từ ​​các chỉ mục trước đó. 

Mỗi cạnh từ một nút đến nút cha của nó sẽ nhân số đóng góp lên 2. Do đó, mỗi nút i đóng góp giá trị ban đầu i nhân với 2 nâng lên khoảng cách từ i đến nút 1 trong cây ngẫu nhiên này. 

Nhiệm vụ sẽ tính toán giá trị kỳ vọng của tổng này trên tất cả các nút. 

Chúng tôi tiến hành như sau.

1. Xác định di là độ sâu của nút i trong cây đệ quy ngẫu nhiên, trong đó nút 1 là gốc. 
2. Biểu thị đáp án cuối cùng là a1 = Σ i · 2^{di}. Điều này hợp lệ vì mỗi khi một giá trị di chuyển một bước về phía nút 1, nó sẽ được nhân với 2, do đó tổng mức khuếch đại chính xác là 2^{distance}. 
3. Lấy kỳ vọng tuyến tính trên tất cả các nút: E[a1] = Σ i · E[2^{di}]. Nhiệm vụ còn lại là tính E[2^{di}] cho mỗi i. 
4. Xác định fi = E[2^{di}]. Trong cây đệ quy ngẫu nhiên, nút i gắn đồng nhất với bất kỳ nút nào trước đó j < i, vì vậy 

fi = 2 · (1/(i − 1)) · Σfj với j < i. 
5. Cho Si = Σ_{j ≤ i} fj. Khi đó phép truy hồi trở thành Si = Si−1 · (1 + 2/(i − 1)). 
6. Viết lại thừa số nhân thành (i + 1)/(i − 1). Kính thiên văn này sạch sẽ: 

Si = ∏_{k=2..i} (k + 1)/(k − 1). 
7. Sản phẩm đơn giản hóa hoàn toàn: 

tử số trở thành 3 · 4 · ... · (i + 1), 

mẫu số trở thành 1 · 2 · ... · (i − 1), 

ước tính Si = i(i + 1)/2. 
8. Khôi phục fi từ Si: fi = Si − Si−1 = i. 
9. Thay thế trở lại mong đợi: 

E[a1] = Σ i · fi = Σ i^2 trên i = 1..n. 
10. Tính tổng các bình phương dạng đóng. 

### Tại sao nó hoạt động 

Toàn bộ mức giảm phụ thuộc vào tính tuyến tính của kỳ vọng và thực tế là các khoản đóng góp nhân lên độc lập dọc theo các cạnh của cây. Cấu trúc ngẫu nhiên chỉ ảnh hưởng đến các nút nằm trên đường dẫn đến 1, chứ không ảnh hưởng đến sự đóng góp độ sâu theo cấp số nhân dự kiến, bởi vì phép đệ quy cho E[2^{độ sâu}] sụp đổ thành một tích số lồng nhau. Khi fi đơn giản hóa thành i, tính ngẫu nhiên biến mất hoàn toàn khỏi biểu thức cuối cùng, chỉ để lại đa thức xác định trong n. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve(n):
    n %= MOD
    return n * (n + 1) % MOD * (2 * n + 1) % MOD * pow(6, MOD - 2, MOD) % MOD

t = int(input())
for _ in range(t):
    n = int(input())
    print(solve(n))
```Mã áp dụng trực tiếp tổng bình phương dạng đóng modulo của số nguyên tố đã cho. Việc cần cẩn thận duy nhất là chia mô đun cho 6, được xử lý bằng cách sử dụng nghịch đảo của Fermat vì mô đun là số nguyên tố. 

Đạo hàm cho thấy tại sao không cần mô phỏng hoặc DP: quá trình ngẫu nhiên chuyển thành một nhận dạng đa thức đơn giản. 

## Ví dụ đã hoạt động 

Xét n = 3. Ban đầu mảng là [1, 2, 3]. Công thức cho 1^2 + 2^2 + 3^2 = 14. 

| tôi | di (ngẫu nhiên) | 2^{di} kỳ vọng | đóng góp i · E[2^{di}] | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 2 | 4 | 
| 3 | biến | 3 | 9 | 

Tổng kết cho 14 bất kể cấu trúc ngẫu nhiên, xác nhận tính nhất quán. 

Với n = 2, chúng ta có một phép toán đơn lẻ xác định: a1 trở thành 5. Công thức cho 1^2 + 2^2 = 5, khớp chính xác. 

| tôi | hiệu ứng hoạt động | đóng góp cuối cùng | 
| --- | --- | --- | 
| 1 | vẫn căn cứ 1 | 1 | 
| 2 | thêm 2·2 vào a1 | 4 | 
| tổng cộng | | 5 | 

Điều này xác nhận rằng ngay cả trong trường hợp không tầm thường nhỏ nhất, công thức vẫn nắm bắt chính xác quá trình ngẫu nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi câu trả lời là một đánh giá đa thức trực tiếp | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp dễ dàng phù hợp trong giới hạn vì thậm chí 10^4 truy vấn đều được trả lời bằng số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve_all(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve(n):
        n %= MOD
        return n * (n + 1) % MOD * (2 * n + 1) % MOD * pow(6, MOD - 2, MOD) % MOD

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(solve(n)))
    return "\n".join(out)

def run(inp: str) -> str:
    return solve_all(inp)

# provided samples (sanity-style)
assert run("1\n1\n") == "1"
assert run("1\n2\n") == "5"

# custom cases
assert run("1\n3\n") == str((1+4+9)%MOD)
assert run("1\n10\n") == str(sum(i*i for i in range(1,11)) % MOD)
assert run("3\n1\n2\n3\n") == "\n".join([str(1), str(5), str(14)])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 1 | Độ ổn định của vỏ cơ sở | 
| n = 2 | 5 | Hoạt động cưỡng bức đơn chính xác | 
| n = 3 | 14 | Tuyên truyền không cần thiết đầu tiên | 
| nhiều truy vấn | tính đúng đắn cho từng trường hợp | Xử lý T và tính độc lập | 

## Vỏ cạnh 

Đối với n = 1, không có phép toán nào xảy ra và a1 vẫn bằng 1. Công thức cho 1^2 = 1, do đó kết quả nhất quán mà không cần bất kỳ xử lý đặc biệt nào. 

Với n = 2, chỉ có đúng một thao tác và không có sự ngẫu nhiên trong việc chọn j. Đạo hàm vẫn được áp dụng vì phép tính độ sâu dự kiến ​​suy biến rõ ràng và kết quả 5 khớp với tính toán trực tiếp. 

Đối với n lớn gần 10^9, mô phỏng trực tiếp sẽ vượt quá giới hạn thời gian ngay lập tức, nhưng biểu thức dạng đóng vẫn ổn định theo số học mô-đun vì nó chỉ phụ thuộc vào đánh giá đa thức và nghịch đảo mô-đun.
