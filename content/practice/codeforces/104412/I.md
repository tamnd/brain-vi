---
title: "CF 104412I - Iron Fist Ketil vs King Canute"
description: "Chúng ta có hai nhóm người đối mặt với nhau trong trận chiến một lần. Một bên có nông dân của Ketil, bên kia có binh lính của Canute."
date: "2026-07-01T00:59:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "I"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 60
verified: true
draft: false
---

[CF 104412I - Iron Fist Ketil vs King Canute](https://codeforces.com/problemset/problem/104412/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai nhóm người đối mặt với nhau trong trận chiến một lần. Một bên có nông dân của Ketil, bên kia có binh lính của Canute. Mỗi người lính Canute cần một lượng nỗ lực cố định để đánh bại: cụ thể là cần chính xác$K$những người nông dân khác nhau để hạ gục một người lính. Một khi nông dân được giao nhiệm vụ chiến đấu với một người lính, nông dân đó không thể chuyển đổi mục tiêu sau đó. 

Câu hỏi đặt ra là liệu Ketil có đủ nông dân để giao ít nhất$K$trong số họ cho mọi người lính trong quân đội của Canute, vì mỗi nông dân chỉ có thể tham gia vào một cuộc chiến. 

Đầu vào mô tả ba giá trị: số lượng nông dân$N$, số lượng binh lính$M$, và số nông dân cần thiết cho mỗi người lính$K$. Đầu ra là một chuỗi quyết định duy nhất cho biết liệu phe của Ketil có thể đánh bại tất cả hay không$M$người lính dưới những ràng buộc này. 

Phạm vi ràng buộc$1 \leq N, M, K \leq 10^6$ngụ ý rằng bất kỳ nghiệm nào cũng phải có thời gian không đổi hoặc cùng lắm là tuyến tính theo một nghĩa rất nhỏ. Bất kỳ mô phỏng chiến đấu hoặc phân bổ theo từng người lính nào đều sẽ không cần thiết và có nguy cơ kém hiệu quả nếu được thực hiện một cách ngây thơ đối với tất cả binh lính và nông dân. Một điều kiện số học trực tiếp được mong đợi. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất hiện khi nghĩ về khía cạnh “phân công theo mỗi người lính” mà không tổng hợp tổng nhu cầu. Ví dụ, người ta có thể cố gắng phân công nông dân tham lam từng người lính một cách không chính xác mà không nhận ra rằng yêu cầu này mang tính chất cộng gộp trên toàn cầu. 

Hãy xem xét đầu vào này:```
N = 3, M = 2, K = 2
```Mỗi người lính cần 2 nông dân, vì vậy tổng số yêu cầu là 4 nông dân, nhưng chỉ có 3 tồn tại. Một nỗ lực tham lam ngây thơ có thể giao 2 nông dân cho người lính đầu tiên và sau đó nhầm tưởng rằng 1 nông dân còn lại là đủ cho người lính thứ hai nếu không theo dõi sự cạn kiệt một cách chính xác. Kết quả đúng là chiến thắng là điều không thể. 

Khó khăn cốt lõi là nhận ra rằng không có cấu trúc tương tác giữa những người lính ngoài việc tiêu thụ một nhóm nông dân chung. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng trận chiến một cách rõ ràng. Với mỗi người lính, chúng tôi sẽ cố gắng phân công$K$nông dân không sử dụng. Điều này sẽ liên quan đến việc lặp đi lặp lại các nông dân, đánh dấu họ là đã sử dụng và tiếp tục cho đến khi tất cả binh sĩ hài lòng hoặc chúng tôi hết nông dân. Trong trường hợp xấu nhất, mỗi nhiệm vụ có thể quét hoặc tìm kiếm trong số những nông dân còn lại, dẫn đến sự phức tạp theo thứ tự$O(N \cdot M)$. Với giá trị lên đến$10^6$, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là danh tính của nông dân không quan trọng, chỉ có số lượng của họ là quan trọng. Mỗi người lính tiêu thụ chính xác$K$nông dân riêng biệt, và không có nông dân nào có thể được tái sử dụng. Điều này biến vấn đề thành một câu hỏi kế toán tài nguyên thuần túy: chúng ta cần kiểm tra xem tổng số nông dân có sẵn có$N$ít nhất là tổng nhu cầu$M \cdot K$. 

Một khi điều này được nhận ra, bài toán sẽ chuyển thành một phép so sánh duy nhất giữa hai số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot M)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị$N$,$M$, Và$K$từ đầu vào. Chúng đại diện cho những nông dân có sẵn, binh lính địch và nông dân cần có cho mỗi người lính. 
2. Tính tổng số nông dân cần thiết để đánh bại tất cả binh lính, đó là$M \times K$. Điều này tổng hợp yêu cầu của mỗi người lính thành một nhu cầu toàn cầu duy nhất. 
3. So sánh giá trị yêu cầu này với$N$. Nếu như$N \geq M \times K$, khi đó có thể phân công các nhóm nông dân rời rạc cho từng người lính. 
4. Đầu ra`"Iron fist Ketil"`nếu điều kiện được giữ, nếu không thì xuất ra`"King Canute"`. 

Điểm quyết định quan trọng là bước 3, trong đó chúng tôi coi toàn bộ chiến trường là vấn đề phân bổ nguồn lực chứ không phải là một chuỗi các cuộc chiến độc lập. 

### Tại sao nó hoạt động 

Mỗi người lính tiêu thụ chính xác$K$nông dân duy nhất và nông dân không thể được tái sử dụng. Điều này tạo ra một yêu cầu phân vùng nghiêm ngặt: tập hợp tất cả nông dân được chỉ định trên tất cả các binh sĩ phải rời rạc và có kích thước chính xác.$M \cdot K$. Vì không có ràng buộc nào khác tồn tại trong các bài tập nên bất kỳ chiến lược hợp lệ nào cũng tương đương với việc chọn$M \cdot K$những người nông dân khác biệt với nhóm$N$. Do đó, tính khả thi chỉ phụ thuộc vào việc có tồn tại đủ số lượng nông dân khác nhau hay không, điều này dẫn đến sự bất bình đẳng đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, M, K = map(int, input().split())

if N >= M * K:
    print("Iron fist Ketil")
else:
    print("King Canute")
```Việc thực hiện mã hóa trực tiếp điều kiện dẫn xuất. phép nhân$M \times K$an toàn trong Python do số nguyên chính xác tùy ý và so sánh là thời gian không đổi. 

Một lỗi triển khai phổ biến là cố gắng mô phỏng các nhiệm vụ hoặc lặp lại các binh sĩ. Điều đó là không cần thiết và sẽ chỉ gây ra nguy cơ xảy ra lỗi logic. Một cạm bẫy tiềm ẩn khác trong các ngôn ngữ khác là tràn số nguyên khi tính toán$M \times K$, vì sản phẩm có thể đạt tới$10^{12}$, nhưng Python tránh vấn đề này một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
N = 12, M = 5, K = 2
```Chúng tôi tính toán số nông dân cần thiết cho mỗi nhóm lính: 

| Bước | Tính toán | Giá trị | 
| --- | --- | --- | 
| Bắt buộc cho mỗi người lính | K | 2 | 
| Tổng số lính | M | 5 | 
| Tổng số yêu cầu | M × K | 10 | 
| Có sẵn | N | 12 | 
| So sánh | 12 ≥ 10 | Đúng | 

Vì nông dân có sẵn vượt quá số nông dân cần thiết nên mỗi người lính có thể được chỉ định 2 nông dân riêng biệt. 

Đầu ra:```
Iron fist Ketil
```Điều này khẳng định rằng công suất dư thừa không còn quan trọng miễn là tổng nhu cầu được đáp ứng. 

### Mẫu 2 

đầu vào:```
N = 1, M = 1, K = 1
```| Bước | Tính toán | Giá trị | 
| --- | --- | --- | 
| Bắt buộc cho mỗi người lính | K | 1 | 
| Tổng số lính | M | 1 | 
| Tổng số yêu cầu | M × K | 1 | 
| Có sẵn | N | 1 | 
| So sánh | 1 ≥ 1 | Đúng | 

Chỉ một người nông dân là đủ để đánh bại một người lính. 

Đầu ra:```
Iron fist Ketil
```Điều này chứng tỏ trường hợp ranh giới trong đó sự bình đẳng vẫn cho phép chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ các phép tính số học và một phép so sánh duy nhất được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó thực hiện tính toán theo thời gian không đổi bất kể kích thước đầu vào và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N, M, K = map(int, input().split())
    return "Iron fist Ketil" if N >= M * K else "King Canute"

# provided samples
assert run("12 5 2") == "Iron fist Ketil"
assert run("1 1 1") == "Iron fist Ketil"
assert run("1 2 2") == "King Canute"

# custom cases
assert run("10 3 3") == "King Canute"
assert run("0 1 1") == "King Canute"
assert run("1000000 1 1000000") == "Iron fist Ketil"
assert run("7 7 1") == "Iron fist Ketil"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 3 3 | Vua Canute | không đủ tổng công suất mặc dù có nhiều binh sĩ | 
| 0 1 1 | Vua Canute | trường hợp cạnh tài nguyên bằng không | 
| 1000000 1 1000000 | Ketil nắm đấm sắt | trường hợp bình đẳng biên tối đa | 
| 7 7 1 | Ketil nắm đấm sắt | nhiều nhiệm vụ độc lập nhỏ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi yêu cầu phù hợp chính xác với những người nông dân sẵn có. Ví dụ:```
N = 6, M = 3, K = 2
```Mô phỏng đưa ra tổng số yêu cầu$3 \times 2 = 6$, chính xác bằng$N$. Thuật toán tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| N | 6 | 
| M × K | 6 | 
| So sánh | 6 ≥ 6 → Đúng | 

Đầu ra là`"Iron fist Ketil"`, khẳng định rằng sự bình đẳng là đủ vì mỗi nông dân có thể được phân công chính xác một lần. 

Một trường hợp khác là khi không có lính hoặc lính tối thiểu nhưng đảm bảo ràng buộc$M \geq 1$, do đó công thức luôn được áp dụng mà không cần phân nhánh đặc biệt.
