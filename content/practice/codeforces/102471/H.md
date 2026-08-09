---
title: "CF 102471H - Vua"
description: "Chúng ta có một dãy (b1,b2,ldots,bn) gồm các số dư khác 0 modulo một số nguyên tố (p). Chuỗi King là một chuỗi con có các giá trị liên tiếp thu được bằng cách nhân với một phần dư cố định khác 0 (q)."
date: "2026-08-09T04:43:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "H"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 390
verified: true
draft: false
---

[CF 102471H - Vua](https://codeforces.com/problemset/problem/102471/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy (b_1,b_2,\ldots,b_n) gồm các thặng dư khác 0 modulo một số nguyên tố (p). Chuỗi King là một chuỗi con có các giá trị liên tiếp thu được bằng cách nhân với một phần dư cố định khác 0 (q). Nói cách khác, sau khi chọn một số vị trí (i_1<i_2<\cdots<i_k), giá trị của chúng phải thỏa mãn 

[ 
b_{i_{j+1}}\equiv q b_{i_j}\pmod p 
] 

cho mỗi cặp liên tiếp. 

Nhiệm vụ là tìm độ dài tối đa có thể. Có một mệnh đề thoát đặc biệt: nếu giá trị lớn nhất nhỏ hơn (n/2), chúng ta chỉ phải in (-1). Nếu không, chúng ta phải in độ dài tối đa thực tế. Câu lệnh ban đầu sử dụng cùng một (n) cho độ dài chuỗi, vì vậy chúng tôi sử dụng (n) xuyên suốt. 

Tính nguyên tố của (p) cho ta một tính chất quan trọng. Mọi (b_i) đều khác 0 modulo (p), nên mọi (b_i) đều có một nghịch đảo nhân. Do đó, bất kỳ hai vị trí riêng biệt nào cũng xác định ngay một tỷ lệ có thể có. Nếu chúng ta quyết định rằng (b_i) và (b_j) là các phần tử liên tiếp của một dãy con King thì tỉ số duy nhất có thể xảy ra là 

[ 
q\equiv b_j b_i^{-1}\pmod p. 
] 

Ràng buộc (n\le 200000), cùng với tổng (n) tối đa (200000), loại trừ mọi thứ bậc hai hoặc bậc ba trong trường hợp bình thường. Một giải pháp quét toàn bộ chuỗi một lần cho mỗi (O(n)) tỷ lệ có thể có sẽ thực hiện (O(n^2)) hoạt động, khoảng (4\cdot10^{10}) lần lặp ở kích thước tối đa. Giới hạn 2 giây về cơ bản yêu cầu công tuyến tính trên một số lần thử không đổi. 

Có một số trường hợp khó xử lý. Đầu tiên, (n=2) là đặc biệt vì hai giá trị bất kỳ khác 0 tạo thành một chuỗi Vua: tỷ lệ của chúng chỉ đơn giản là (b_2b_1^{-1}). Vì vậy đáp án luôn là (2). Ví dụ: với (n=2,p=7) và dãy (3,5), câu trả lời là (2), không phải (-1). 

Trường hợp ranh giới thứ hai xuất phát từ ngưỡng phân số. Điều kiện ít nhất là độ dài (n/2), do đó so sánh số nguyên là (2L\ge n). Với (n=5), một chuỗi có độ dài (3) đủ điều kiện vì (3\ge2.5). Ví dụ,```
1
5 7
2 4 5 6 8
```có đáp án (3), sử dụng (2,4,8) với tỉ số (2). Việc thực hiện bất cẩn bằng cách sử dụng`length >= n // 2`sẽ coi chiều dài (2) là đủ không chính xác. 

Các giá trị lặp lại cũng quan trọng. Nếu tất cả các giá trị đều bằng nhau thì (q=1), do đó toàn bộ mảng là một chuỗi Vua. Ví dụ,```
1
5 7
3 3 3 3 3
```có câu trả lời (5). Việc triển khai giả định (q\ne1) sẽ từ chối một chuỗi tối đa hợp lệ. 

Cuối cùng, phép chia mô-đun phải được thực hiện bằng cách sử dụng phép chia mô-đun nghịch đảo chứ không phải phép chia số nguyên thông thường. Đối với (p=7), tỷ lệ giữa (2) và (5) là (5\cdot2^{-1}\equiv5\cdot4\equiv6\pmod7), không phải (5/2) như một phép toán số nguyên thông thường. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất bắt đầu bằng cách chọn hai phần tử đầu tiên của dãy con Vua. Giả sử chúng xảy ra ở vị trí (i<j). Họ xác định duy nhất 

[ 
q=b_jb_i^{-1}\pmod p. 
] 

Khi (q) được cố định, chúng ta có thể mở rộng dãy con một cách tham lam. Bắt đầu từ (b_j), quét sang phải và lấy phần tử đầu tiên bằng (qb_j), sau đó là phần tử đầu tiên bằng (q^2b_j), v.v. Ý tưởng tương tự hoạt động ngược lại bằng cách sử dụng (q^{-1}). 

Điều này đúng vì đối với một phần tử ban đầu cố định và tỷ lệ cố định, việc lấy lần xuất hiện tiếp theo sớm nhất có thể không bao giờ có thể ảnh hưởng đến các lựa chọn trong tương lai. Mỗi phần tử sau này có ít nhất khoảng trống để tiếp tục chuỗi. 

Vấn đề là số lượng cặp bắt đầu có thể. Có các cặp (O(n^2)) và kiểm tra từng cặp bằng cách quét chi phí chuỗi (O(n)), đưa ra (O(n^3)) trong trường hợp xấu nhất. Tại (n=200000), đó là thứ tự của (8\cdot10^{15}) thao tác quét cơ bản, điều này hoàn toàn không khả thi. 

Điều kiện đầu ra đặc biệt sẽ thay đổi vấn đề. Chúng tôi chỉ quan tâm đến các trường hợp dãy con King chứa ít nhất một nửa mảng. Dãy con lớn như vậy dày đặc bên trong dãy ban đầu. Thay vì thử mọi cặp bắt đầu có thể, hãy chọn ngẫu nhiên một vị trí mảng (x). Nếu (x) thuộc dãy con King có độ dài ít nhất là (n/2), thì đó là một mỏ neo hữu ích. Giải pháp ngẫu nhiên dự định sử dụng các vị trí rất gần nhau (x+1) và (x+2) để thu được tỷ lệ ứng cử viên, sau đó quét toàn bộ mảng để tìm từng ứng cử viên. 

Đây là cách rút gọn quan trọng: chúng ta không cần xác định tỷ lệ một cách xác định. Chúng ta chỉ cần một cơ hội có xác suất không đổi để tạo ra tỷ lệ chính xác vì thuật toán có thể lặp lại thí nghiệm nhiều lần. Cách tiếp cận tiêu chuẩn được chấp nhận lặp lại thí nghiệm này 100 lần. 

Đối với tỷ lệ ứng cử viên cố định (q), bản thân quá trình quét là tuyến tính. Nếu điểm neo được chọn ở vị trí (x), chúng tôi sẽ mở rộng về phía sau bằng cách sử dụng (q^{-1}), sau đó tiến về phía trước bằng cách sử dụng (q). Lần xuất hiện khớp sớm nhất luôn là an toàn để thực hiện, do đó, độ dài kết quả là chuỗi con King tối đa chứa điểm neo đó cho tỷ lệ này. 

Mọi câu trả lời được báo cáo đều được xác minh độc lập bằng cấu trúc này, vì vậy việc chọn ngẫu nhiên chỉ có thể gây ra kết quả âm tính giả chứ không bao giờ gây ra kết quả dương tính giả. Nếu thuật toán in ra độ dài (L), thì các phần tử thực sự được thu thập sẽ tạo thành một dãy con King hợp lệ có độ dài (L). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(1)) | Quá chậm | 
| Ngẫu nhiên tối ưu | (O(Kn)), (K=100) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n), (p) và mảng. Vì mọi (b_i) nằm giữa (1) và (p-1), nên mọi giá trị đều có nghịch đảo mô đun. 
2. Đối với các mảng rất nhỏ, hãy liệt kê trực tiếp các cặp. Điều này tránh sự ngẫu nhiên không cần thiết xung quanh các trường hợp biên nhỏ và cho kết quả chính xác. Đối với một cặp vị trí (i<j), hãy tính 

[ 
q=b_jb_i^{-1}\pmod p 
] 

và tính toán chuỗi con King dài nhất bằng tỷ lệ đó. 

1. Đối với các mảng lớn hơn, hãy lặp lại thí nghiệm ngẫu nhiên 100 lần. Chọn một vị trí ngẫu nhiên thống nhất (x). 
2. Nếu (x+1\le n), sử dụng 

[ 
q=b_{x+1}b_x^{-1}\pmod p 
] 

như một tỷ lệ ứng cử viên. Ứng cử viên này đặc biệt hữu ích khi (x) và (x+1) là các phần tử liên tiếp của dãy Vua mong muốn. 

1. Nếu (x+2\le n), sử dụng tương tự 

[ 
q=b_{x+2}b_x^{-1}\pmod p. 
] 

Ứng cử viên thứ hai xử lý tình huống trong đó các phần tử hữu ích được phân tách bằng một phần tử mảng bị loại bỏ. Đây là một phần của chiến lược ngẫu nhiên được sử dụng cho vấn đề này. 

1. Với mọi tỷ lệ ứng viên (q), hãy tính (q^{-1}) theo định lý nhỏ Fermat: 

[ 
q^{-1}\equiv q^{p-2}\pmod p. 
] 

Vì (p) là số nguyên tố và (q\ne0) nên nghịch đảo này luôn tồn tại.

1. Bắt đầu từ (b_x), quét sang trái. Giữ giá trị mong đợi hiện tại bằng giá trị hiện tại nhân với (q^{-1}). Bất cứ khi nào phần tử được quét bằng giá trị mong đợi, hãy lấy nó và cập nhật giá trị mong đợi. Lấy lần xuất hiện đầu tiên có thể xảy ra là tham lam và tối đa hóa số phần tử mà chúng ta vẫn có thể sử dụng. 
2. Bắt đầu lại từ vị trí neo thứ hai và quét sang phải. Bất cứ khi nào một phần tử bằng giá trị hiện tại nhân với (q), hãy lấy nó. Cộng số phần tử thu được ở cả hai bên và hai phần tử neo. 
3. Cập nhật câu trả lời hay nhất. Cuối cùng, xuất ra độ dài tốt nhất nếu thỏa mãn (2\cdot\text{best}\ge n). Nếu không thì xuất ra (-1). 

### Tại sao nó hoạt động 

Đối với mọi tỷ lệ ứng cử viên cố định (q), quá trình quét tham lam duy trì tính bất biến rằng các phần tử được chọn tạo thành một chuỗi Vua hợp lệ và phần tử được chọn cuối cùng là phần tử sớm nhất có thể có giá trị được yêu cầu. Việc thay thế bất kỳ phần tử đã chọn nào bằng một lần xuất hiện trước đó không thể làm giảm tập hợp các vị trí có sẵn sau đó, do đó quá trình quét tham lam sẽ thu được chuỗi con tối đa tương thích với điểm neo và tỷ lệ đó. 

Giả sử một dãy con King tối ưu có độ dài ít nhất là (n/2). Các phần tử của nó chiếm ít nhất một nửa số vị trí ban đầu, do đó, một vị trí ngẫu nhiên thống nhất có xác suất không đổi để chạm vào chuỗi con này. Khi một mỏ neo hữu ích được chọn, các phần tử lân cận sẽ cung cấp tỷ lệ ứng cử viên với xác suất không đổi theo chiến lược ngẫu nhiên dự kiến. Việc lặp lại thí nghiệm 100 lần khiến xác suất bỏ sót mọi ứng cử viên hữu ích là cực kỳ nhỏ. Đây là lý do tại sao yêu cầu (n/2) bất thường của bài toán lại cho phép giải pháp thời gian tuyến tính ngẫu nhiên. 

Thuật toán không thể đưa ra câu trả lời khẳng định không hợp lệ vì mọi độ dài ứng cử viên đều xuất phát từ dãy con King được xây dựng rõ ràng. Tính ngẫu nhiên chỉ ảnh hưởng đến việc liệu một chuỗi có đủ dài hay không được phát hiện. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

TRIALS = 100
SMALL = 50

def fixed_ratio_length(a, p, left, right, q):
    """
    a is 0-indexed.
    left and right are two consecutive chosen positions,
    and a[right] == q * a[left] (mod p).

    Return the longest King subsequence with ratio q that
    contains both anchor positions.
    """
    inv_q = pow(q, p - 2, p)

    length = 2
    cur = a[left]

    for i in range(left - 1, -1, -1):
        if a[i] == cur * inv_q % p:
            cur = a[i]
            length += 1

    cur = a[right]

    for i in range(right + 1, len(a)):
        if a[i] == cur * q % p:
            cur = a[i]
            length += 1

    return length

def exact_small(a, p):
    n = len(a)
    best = 1

    for i in range(n):
        inv_ai = pow(a[i], p - 2, p)

        for j in range(i + 1, n):
            q = a[j] * inv_ai % p
            best = max(best, fixed_ratio_length(a, p, i, j, q))

    return best

def solve_case(n, p, a, rng):
    if n <= SMALL:
        return exact_small(a, p)

    best = 1

    for _ in range(TRIALS):
        x = rng.randrange(n)

        if x + 1 < n:
            q = a[x + 1] * pow(a[x], p - 2, p) % p
            best = max(best, fixed_ratio_length(a, p, x, x + 1, q))

        if x + 2 < n:
            q = a[x + 2] * pow(a[x], p - 2, p) % p
            best = max(best, fixed_ratio_length(a, p, x, x + 2, q))

    if 2 * best >= n:
        return best
    return -1

def main():
    rng = random.Random()

    T = int(input())
    out = []

    for _ in range(T):
        n, p = map(int, input().split())
        a = list(map(int, input().split()))
        out.append(str(solve_case(n, p, a, rng)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`fixed_ratio_length`chức năng thực hiện quét tham lam lõi. Hai vị trí neo đã được biết là thành viên liên tiếp cho tỷ lệ ứng cử viên, do đó độ dài ban đầu là`2`. Việc quét ngược sử dụng`inv_q`, trong khi quá trình quét tiến sử dụng`q`. 

biểu hiện`pow(x, p - 2, p)`là lũy thừa mô-đun của Python. Nó tính nghịch đảo của`x`modulo số nguyên tố (p) trong thời gian (O(\log p)). Không có vấn đề tràn số nguyên trong Python và mô-đun được áp dụng trong quá trình lũy thừa và nhân mô-đun. 

Sự so sánh`2 * best >= n`cố tình tránh số học dấu phẩy động. Điều này xử lý chính xác cả giá trị chẵn và lẻ của (n). Ví dụ: khi (n=5), độ dài (3,4,5) đủ điều kiện, trong khi độ dài (2) thì không. 

các`SMALL`nhánh không cần thiết cho thuật toán tiệm cận. Nó làm cho việc triển khai mang tính quyết định đối với các đầu vào nhỏ, trong đó lực lượng vũ phu khối không tốn kém và loại bỏ hành vi ranh giới khó xử đối với các mảng nhỏ. 

Phần ngẫu nhiên sử dụng 100 thử nghiệm, phù hợp với chiến lược giải pháp tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét```
6 1000000007
1 1 2 4 8 16
```Dãy số (1,2,4,8,16) là dãy con Vua có tỉ số (2) nên đáp án đúng là (5). 

Giả sử một thử nghiệm ngẫu nhiên chọn vị trí (2), sử dụng chỉ số dựa trên một. Khi đó (a_2=1), (a_3=2), nên tỷ lệ ứng cử viên đầu tiên là 

[ 
q=2\cdot1^{-1}\equiv2. 
] 

Quá trình quét tiến hành như sau. 

| Vị trí | Giá trị | Dự kiến ​​trong khi quét | Hành động | Chiều dài | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | Neo | 2 | 
| 1 | 1 | (1\cdot2^{-1}) | Đi | 3 | 
| 3 | 2 | (1\cdot2) | Đi | 4 | 
| 4 | 4 | (2\cdot2) | Đi | 5 | 
| 5 | 8 | (4\cdot2) | Đi | 6 | 
| 6 | 16 | (8\cdot2) | Đi | 7 | 

Bảng minh họa lý do tại sao việc triển khai phải cẩn thận với số lượng neo. Quá trình quét ngược sẽ thêm thông tin trước đó`1`, trong khi quá trình quét tiến bắt đầu từ điểm neo thứ hai`2`. Do đó, chuỗi thực tế được tìm thấy là (1,1,2,4,8,16), có độ dài (6), không phải (5). Đầu vào hoàn chỉnh thực sự có sáu giá trị và đây chính là chuỗi King có tỷ lệ (2) sau khi chọn hai giá trị bằng nhau đầu tiên? Không. Hai giá trị đầu tiên là (1,1), do đó cặp đó có tỷ lệ (1), trong khi chuỗi từ giá trị thứ hai trở đi có tỷ lệ (2). Do đó, ứng cử viên (q=2) không thể bao gồm cả hai vị trí đầu tiên và so sánh ngược sẽ loại bỏ giá trị đầu tiên vì (1\ne1\cdot2^{-1}\pmod p). Do đó, dấu vết chính xác là: 

| Vị trí | Giá trị | Dự kiến ​​| Hành động | Chiều dài | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | Neo | 2 | 
| 1 | 1 | (500000004) | Bỏ qua | 2 | 
| 3 | 2 | 2 | Đi | 3 | 
| 4 | 4 | 4 | Đi | 4 | 
| 5 | 8 | 8 | Đi | 5 | 
| 6 | 16 | 16 | Đi | 6 | 

Độ dài kết quả là (5), sử dụng các vị trí (2,3,4,5,6). Vì (2\cdot5\ge6) nên đáp án là (5). 

### Mẫu 2 

Hãy xem xét```
6 1000000007
597337906 816043578 617563954 668607211 89163513 464203601
```Không có dãy con King nào có độ dài ít nhất là (3), nên đáp án là (-1). 

Phép thử có thể chọn một vị trí tùy ý và xây dựng một hoặc hai tỷ lệ ứng cử viên. Sau đó, mỗi ứng cử viên sẽ được quét qua toàn bộ mảng. Trạng thái quan trọng là giá trị yêu cầu hiện tại. 

| Dùng thử | Neo | Nguồn ứng viên | Ứng viên (q) | Độ dài tốt nhất được tìm thấy | 
| --- | --- | --- | --- | --- | 
| 1 | ngẫu nhiên | (x,x+1) | tỷ lệ của cặp | 2 | 
| 1 | ngẫu nhiên | (x,x+2) | tỷ lệ của cặp | 2 | 
| 2 | ngẫu nhiên | (x,x+1) | tỷ lệ của cặp | 2 | 
| 2 | ngẫu nhiên | (x,x+2) | tỷ lệ của cặp | 2 | 
| ... | ... | ... | ... | 2 | 

Độ dài ứng cử viên của (2) luôn hợp lệ vì bất kỳ hai dư lượng khác 0 nào cũng xác định một tỷ lệ. Vì (2\cdot2<6) nên kết quả cuối cùng là (-1). 

Ví dụ này thể hiện một đặc tính quan trọng của phương pháp ngẫu nhiên: các ứng viên không thành công không gây ra các câu trả lời tích cực sai. Đơn giản là họ không đạt được ngưỡng yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(Kn+K\log p)) | Mỗi thử nghiệm (K=100) thực hiện một số lần quét tuyến tính và đảo ngược mô-đun không đổi | 
| Không gian | (O(n)) | Mảng được lưu trữ rõ ràng | 

Tổng (n) trên tất cả các trường hợp thử nghiệm nhiều nhất là (200000), do đó số lần quét tuyến tính vẫn tỷ lệ thuận với kích thước đầu vào chung nhân với số lượng thử nghiệm ngẫu nhiên không đổi. Việc triển khai Python cũng sử dụng số học mô-đun chính xác và chỉ lưu trữ mảng đầu vào cộng với trạng thái tạm thời có kích thước không đổi. 

Bản chất ngẫu nhiên nên được hiểu là một phần của giải pháp dự kiến ​​chứ không phải là sự tối ưu hóa ngẫu nhiên. Giải pháp được công bố tiêu chuẩn sử dụng cùng chiến lược (O(Kn)) với (K=100). 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới sử dụng logic giải pháp tương tự. Các trường hợp nhỏ được giải quyết triệt để, điều này làm cho các thử nghiệm ranh giới tùy chỉnh mang tính xác định. Trường hợp hoàn toàn bằng nhau lớn cũng có tính xác định vì mọi tỷ lệ ứng viên đều là (1).```python
# helper: run solution on input string, return output string
import io
import random
import sys

TRIALS = 100
SMALL = 50

def fixed_ratio_length(a, p, left, right, q):
    inv_q = pow(q, p - 2, p)

    length = 2
    cur = a[left]

    for i in range(left - 1, -1, -1):
        if a[i] == cur * inv_q % p:
            cur = a[i]
            length += 1

    cur = a[right]

    for i in range(right + 1, len(a)):
        if a[i] == cur * q % p:
            cur = a[i]
            length += 1

    return length

def exact_small(a, p):
    best = 1

    for i in range(len(a)):
        inv_ai = pow(a[i], p - 2, p)

        for j in range(i + 1, len(a)):
            q = a[j] * inv_ai % p
            best = max(best, fixed_ratio_length(a, p, i, j, q))

    return best

def solve_case(n, p, a, rng):
    if n <= SMALL:
        best = exact_small(a, p)
    else:
        best = 1

        for _ in range(TRIALS):
            x = rng.randrange(n)

            if x + 1 < n:
                q = a[x + 1] * pow(a[x], p - 2, p) % p
                best = max(
                    best,
                    fixed_ratio_length(a, p, x, x + 1, q)
                )

            if x + 2 < n:
                q = a[x + 2] * pow(a[x], p - 2, p) % p
                best = max(
                    best,
                    fixed_ratio_length(a, p, x, x + 2, q)
                )

    return str(best if 2 * best >= n else -1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    rng = random.Random(123456789)

    try:
        T = int(sys.stdin.readline())
        ans = []

        for _ in range(T):
            n, p = map(int, sys.stdin.readline().split())
            a = list(map(int, sys.stdin.readline().split()))
            ans.append(solve_case(n, p, a, rng))

        return "\n".join(ans)
    finally:
        sys.stdin = old_stdin

# Provided samples
sample = """\
4
6 1000000007
1 1 2 4 8 16
6 1000000007
597337906 816043578 617563954 668607211 89163513 464203601
5 1000000007
2 4 5 6 8
5 1000000007
2 4 5 6 7
"""

assert run(sample) == "5\n-1\n3\n-1", "provided samples"

# Minimum-size input: every pair is a King sequence.
assert run("""\
1
2 7
3 5
""") == "2", "minimum n"

# All equal values: q = 1, so the whole array is valid.
assert run("""\
1
5 7
3 3 3 3 3
""") == "5", "all equal values"

# Odd n: for n = 5, length 3 qualifies, while length 2 does not.
assert run("""\
1
5 1000000007
2 4 5 6 8
""") == "3", "odd threshold"

# No qualifying subsequence.
assert run("""\
1
5 1000000007
2 4 5 6 7
""") == "-1", "below threshold"

# Maximum-size input. All values are equal, so the answer is n.
n = 200000
large_input = "1\n{} 1000000007\n{}\n".format(n, "7 " * (n - 1) + "7")
assert run(large_input) == str(n), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 7 / 3 5`|`2`| Trường hợp kích thước tối thiểu và thực tế là hai giá trị khác 0 bất kỳ tạo thành chuỗi King | 
|`5 7 / 3 3 3 3 3`|`5`| Tỷ lệ (q=1) và giá trị lặp lại | 
|`5 1000000007 / 2 4 5 6 8`|`3`| Ngưỡng lẻ (n) và ngưỡng (2L\ge n) | 
|`5 1000000007 / 2 4 5 6 7`|`-1`| Trường hợp không tồn tại chuỗi nửa độ dài được yêu cầu | 
| (n=200000), tất cả các giá trị`7`|`200000`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Với (n=2), câu trả lời luôn là (2). Với```
1
2 7
3 5
```tỷ lệ là (5\cdot3^{-1}\equiv5\cdot5\equiv4\pmod7), vì vậy (3,5) là dãy Vua. Nhánh trường hợp nhỏ chính xác của việc triển khai tìm thấy cặp này trực tiếp. 

Để có giá trị bằng nhau, lấy```
1
5 7
3 3 3 3 3
```Việc chọn (q=1) sẽ cho ra (1\cdot3\equiv3\pmod7), do đó mọi cặp liên tiếp đều thỏa mãn quy tắc. Tối đa là (5) và (2\cdot5\ge5), do đó đầu ra là`5`. 

Đối với ngưỡng lẻ, hãy xem xét```
1
5 1000000007
2 4 5 6 8
```Dãy con (2,4,8) có tỉ số (2) nên độ dài của nó là (3). Không có độ dài (4) dãy con King, nhưng (3) là đủ vì (2\cdot3=6\ge5). Đầu ra đúng là`3`. Đây là lý do tại sao việc thực hiện sử dụng`2 * best >= n`thay vì`best >= n // 2`. 

Đối với một chuỗi dưới ngưỡng,```
1
5 1000000007
2 4 5 6 7
```dãy con King dài nhất có độ dài (2). Hai phần tử bất kỳ có thể xác định một tỷ lệ, nhưng không có tỷ lệ nào hỗ trợ ba phần tử theo thứ tự bắt buộc. Vì (2\cdot2=4<5), đầu ra là`-1`. 

Ở kích thước đầu vào tối đa, hãy xem xét (200000) bản sao của (7). Tỷ lệ (q=1) có tác dụng xuyên suốt mảng, vì vậy câu trả lời là (200000). Thuật toán không cần lưu trữ bất kỳ bảng lập trình động nào được lập chỉ mục theo giá trị hoặc tỷ lệ. Nó chỉ giữ mảng và quét liên tục nên bộ nhớ vẫn còn (O(n)). 

Ranh giới nghịch đảo mô đun cũng an toàn khi (q=1). Công thức Fermat cho (1^{p-2}\equiv1), do đó quá trình quét lùi và quét tiến tiếp tục một cách chính xác thông qua các giá trị bằng nhau. Mọi giá trị đầu vào đều khác 0 modulo (p), do đó không bao giờ yêu cầu nghịch đảo của 0.
