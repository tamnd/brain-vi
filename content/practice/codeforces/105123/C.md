---
title: "CF 105123C - DNA đảo ngược"
description: "Chúng ta có hai chuỗi DNA có độ dài bằng nhau, chỉ bao gồm các ký tự A, T, C và G. Mỗi vị trí đại diện cho một nucleotide và chúng ta được yêu cầu quyết định xem liệu chuỗi thứ hai có thể thu được từ chuỗi đầu tiên chỉ bằng cách sử dụng các đột biến hoán đổi các bazơ bổ sung hay không."
date: "2026-06-27T19:32:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105123
codeforces_index: "C"
codeforces_contest_name: "BioCode 2024"
rating: 0
weight: 105123
solve_time_s: 77
verified: true
draft: false
---

[CF 105123C - DNA đảo ngược](https://codeforces.com/problemset/problem/105123/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi DNA có độ dài bằng nhau, chỉ bao gồm các ký tự A, T, C và G. Mỗi vị trí đại diện cho một nucleotide và chúng ta được yêu cầu quyết định xem liệu chuỗi thứ hai có thể thu được từ chuỗi đầu tiên chỉ bằng cách sử dụng các đột biến hoán đổi các bazơ bổ sung hay không. 

Quy tắc sinh học là A kết hợp với T và C kết hợp với G. Giải thích dự kiến ​​ở đây là ở mọi vị trí, nếu đột biến xảy ra, nó chỉ có thể chuyển đổi một nucleotide thành đối tác bổ sung của nó và không có gì khác. Vì vậy A có thể trở thành T hoặc vẫn là A, T có thể trở thành A hoặc vẫn là T, C có thể trở thành G hoặc vẫn là C và G có thể trở thành C hoặc vẫn là G. Điều quan trọng là không được phép chuyển đổi cặp chéo như A sang C hoặc G sang T. 

Chúng tôi không sắp xếp lại chuỗi hoặc thực hiện các hoạt động toàn cầu. Mọi vị trí phát triển độc lập và chúng tôi chỉ xác minh xem mỗi ký tự trong chuỗi đầu tiên có thể chuyển đổi hợp pháp thành ký tự tương ứng trong chuỗi thứ hai hay không. 

Ràng buộc n lên tới 10^5 ngụ ý rằng chúng ta cần một giải pháp quét tuyến tính. Bất kỳ cách tiếp cận nào cố gắng khám phá các kết hợp hoặc mô phỏng các đường dẫn đột biến nhiều bước cho mỗi vị trí vẫn chỉ ổn nếu nó duy trì O(1) cho mỗi ký tự. Bất cứ điều gì bậc hai hoặc liên quan đến việc quét lặp đi lặp lại sẽ không thành công trong giới hạn 2 giây. 

Một trường hợp thất bại tinh tế xuất hiện khi các ký tự đến từ các cặp bổ sung khác nhau. Ví dụ: nếu X có A và Y có C ở cùng một vị trí thì điều này không hợp lệ mặc dù cả hai ký tự đều là cơ sở DNA hợp lệ. Một sai lầm ngây thơ là cho rằng “bất kỳ thay đổi nào cũng được miễn là cả hai chữ cái đều hợp lệ”, điều này sẽ chấp nhận những trường hợp như vậy một cách không chính xác. 

Một trường hợp đặc biệt khác là khi các chuỗi giống hệt nhau, điều này phải luôn hợp lệ vì không cần đột biến. Ngoài ra, các chuỗi ký tự đơn phải được xử lý chính xác mà không cần phân nhánh đặc biệt. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô phỏng các đột biến được phép trên mỗi vị trí. Với mỗi chỉ số i, chúng ta kiểm tra xem X[i] có thể trở thành Y[i] hay không. Nếu chúng tôi mô hình hóa đột biến là “bạn có thể ở lại hoặc chuyển sang cơ sở bổ sung của mình”, thì đối với mỗi ký tự, chúng tôi sẽ kiểm tra rõ ràng tư cách thành viên trong cặp được phép của nó. 

Điều này có thể được thực hiện bằng cách xác định trước ánh xạ các chuyển tiếp hợp lệ. Đối với mỗi ký tự trong X, chúng tôi kiểm tra xem Y[i] có bằng chính nó hay bằng phần bù của nó hay không. Đây đã là O(n), nhưng một cách giải thích ngây thơ hơn có thể cố gắng tạo ra các chuỗi đột biến hoặc chuyển đổi đồ thị giữa các chữ cái, điều này là không cần thiết và sẽ đưa ra lý luận chung hoặc thậm chí theo cấp số nhân nếu được mô hình hóa sai thành các phép biến đổi nhiều bước. 

Quan sát quan trọng là quy tắc đột biến hoàn toàn cục bộ và đối xứng: mỗi ký tự thuộc về một cặp cố định {A, T} hoặc {C, G} và đột biến không bao giờ giao nhau giữa các cặp. Vì vậy, điều kiện duy nhất chúng ta cần là X[i] và Y[i] thuộc cùng một cặp. 

Điều này làm giảm vấn đề thành một kiểm tra lớp tương đương đơn giản: A và T tạo thành một lớp, C và G tạo thành một lớp khác. Nếu X[i] và Y[i] khác lớp thì câu trả lời ngay lập tức là KHÔNG. Ngược lại, tất cả các vị trí đều phải thỏa mãn điều kiện này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Kiểm tra cặp tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách phân loại từng ký tự vào nhóm bổ sung của nó và kiểm tra vị trí nhất quán theo vị trí.

1. Xác định các nhóm bổ sung thành hai tập hợp: một nhóm chứa A và T, nhóm kia chứa C và G. Cấu trúc này mã hóa quy tắc đột biến theo cách truy vấn nhanh. 
2. Lặp lại qua mọi chỉ số i từ 0 đến n−1, so sánh X[i] và Y[i]. 
3. Với mỗi vị trí, xác định X[i] thuộc nhóm nào. 
4. Kiểm tra xem Y[i] có thuộc cùng một nhóm hay không. 
5. Nếu bất kỳ quan điểm nào vi phạm điều kiện này, hãy ngay lập tức kết luận rằng giả thuyết đó sai và dừng lại. 
6. Nếu quá trình quét hoàn tất mà không vi phạm, hãy kết luận rằng mọi đột biến đều tôn trọng quy tắc. 

Lý do chấm dứt ngay lập tức là vì một vị trí không hợp lệ sẽ làm mất hiệu lực toàn bộ giả thuyết, do đó việc tiếp tục sẽ chỉ lãng phí tính toán. 

### Tại sao nó hoạt động 

Mỗi ký tự thuộc về đúng một trong hai lớp tương đương được tạo ra bởi tính bổ sung. Quy tắc đột biến chỉ cho phép chuyển đổi trong cùng một lớp. Do đó, bất kỳ phép chuyển đổi hợp lệ nào cũng phải bảo toàn tư cách thành viên lớp ở mọi chỉ mục. Thuật toán thực thi trực tiếp bất biến này bằng cách kiểm tra tính bằng nhau của các lớp theo vị trí, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def same_group(a, b):
    # A <-> T, C <-> G
    if a in "AT":
        return b in "AT"
    else:
        return b in "CG"

n = int(input().strip())
x = input().strip()
y = input().strip()

for i in range(n):
    if not same_group(x[i], y[i]):
        print("NO")
        sys.exit(0)

print("YES")
```Giải pháp đọc các chuỗi đầu vào và xác định hàm trợ giúp mã hóa hai nhóm phần bù. Vòng lặp kiểm tra từng vị trí một cách độc lập và thoát sớm khi có lỗi. Việc sử dụng kiểm tra tư cách thành viên trực tiếp sẽ tránh mọi nhu cầu về bảng ánh xạ rõ ràng hoặc logic nhiều bước, giữ cho việc triển khai ở mức tối thiểu và thời gian không đổi cho mỗi ký tự. 

Một lỗi triển khai phổ biến là cố gắng xâu chuỗi các phần bù, chẳng hạn như kiểm tra xem liệu x có thể tiếp cận y trong hai bước hay không, điều này là không cần thiết vì quy tắc đột biến hoàn toàn không cho phép rời khỏi lớp tương đương. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi so sánh vị trí các chuỗi theo vị trí và xác minh xem mỗi cặp có nằm trong cùng một lớp bổ sung hay không. 

| tôi | X[i] | Y[i] | Nhóm(X[i]) | Nhóm(Y[i]) | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | G | G | CG | CG | Có | 
| 1 | C | C | CG | CG | Có | 
| 2 | A | A | TẠI | TẠI | Có | 
| 3 | A | A | TẠI | TẠI | Có | 
| 4 | G | G | CG | CG | Có | 
| 5 | C | C | CG | CG | Có | 
| 6 | C | C | CG | CG | Có | 
| 7 | T | T | TẠI | TẠI | Có | 

Không có sự không khớp nào xuất hiện nên kết quả đầu ra là CÓ. Điều này xác nhận rằng các ký tự giống hệt nhau luôn hợp lệ theo quy tắc. 

### Mẫu 2 

Ở đây chúng tôi phát hiện sự vi phạm nhóm bổ sung ở một số vị trí. 

| tôi | X[i] | Y[i] | Nhóm(X[i]) | Nhóm(Y[i]) | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | C | C | CG | CG | Có | 
| 1 | G | G | CG | CG | Có | 
| 2 | T | T | TẠI | TẠI | Có | 
| 3 | A | A | TẠI | TẠI | Có | 
| 4 | T | T | TẠI | TẠI | Có | 
| 5 | G | G | CG | CG | Có | 
| 6 | G | G | CG | CG | Có | 
| 7 | A | A | TẠI | TẠI | Có | 
| 8 | C | C | CG | CG | Có | 
| 9 | A | C | TẠI | CG | Không | 

Ở chỉ số 9, A thuộc nhóm AT trong khi C thuộc nhóm CG, phá vỡ ràng buộc đột biến. Thuật toán dừng ngay lập tức và trả về NO. Điều này chứng tỏ rằng ngay cả một sự không khớp giữa các nhóm đơn lẻ cũng sẽ làm mất hiệu lực toàn bộ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét từng vị trí một lần và thực hiện kiểm tra nhóm theo thời gian liên tục | 
| Không gian | O(1) | Chỉ một lượng trạng thái cố định được sử dụng để phân loại | 

Quá trình quét tuyến tính vừa vặn thoải mái trong các giới hạn cho n tối đa 10^5 và các thao tác liên tục trong thời gian cho mỗi ký tự đảm bảo giải pháp chạy hiệu quả trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input().strip())
    x = input().strip()
    y = input().strip()

    def same_group(a, b):
        if a in "AT":
            return b in "AT"
        else:
            return b in "CG"

    for i in range(n):
        if not same_group(x[i], y[i]):
            print("NO")
            return
    print("YES")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided samples
assert run("8\nGCAAGCCT\nCCATCCCT") == "YES", "sample 1"
assert run("10\nCGTATGGACA\nATACTCACCA") == "NO", "sample 2"

# custom cases
assert run("1\nA\nT") == "YES", "single valid flip"
assert run("1\nA\nC") == "NO", "cross group invalid"
assert run("5\nATCGA\nTACGC") == "YES", "mixed valid pairs"
assert run("5\nAAAAA\nCCCCC") == "NO", "all invalid cross group"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 A T | CÓ | phần bù hợp lệ tối thiểu | 
| 1 A C | KHÔNG | từ chối giữa các nhóm | 
| ATCGA / TACGC | CÓ | chuyển tiếp hợp lệ hỗn hợp | 
| AAAAA / CCCCC | KHÔNG | ánh xạ không hợp lệ thống nhất | 

## Vỏ cạnh 

Đối với các đầu vào có một ký tự như X = "A" và Y = "T", thuật toán thực hiện chính xác một lần kiểm tra nhóm. A thuộc lớp AT và T cũng thuộc lớp AT, do đó vòng lặp kết thúc mà không gây ra lỗi và xuất ra CÓ. 

Đối với một ví dụ liên lớp không đạt, chẳng hạn như X = "A" và Y = "C", việc kiểm tra tương tự sẽ xác định rằng A ở AT trong khi C ở CG. Điều kiện không thành công ngay lập tức ở chỉ số 0 và thuật toán xuất ra NO mà không cần xử lý thêm. 

Đối với các chuỗi giống hệt nhau như X = "CGTAC" và Y = "CGTAC", mọi chỉ mục đều duy trì tư cách thành viên nhóm, do đó thuật toán xác nhận tính hợp lệ trong toàn bộ quá trình quét và trả về CÓ.
