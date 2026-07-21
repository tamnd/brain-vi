---
title: "CF 103637B - BSUIR Mở X"
description: "Chúng ta được cung cấp một tập hợp các chuỗi, mỗi chuỗi đại diện cho tên mã của một nhóm tác vụ. Từ bộ sưu tập này, chúng ta phải chọn chính xác hai chuỗi riêng biệt và chúng ta được phép đặt chúng theo một trong hai thứ tự, nối chuỗi này với chuỗi kia."
date: "2026-07-02T22:18:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103637
codeforces_index: "B"
codeforces_contest_name: "2019-2020 10th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 103637
solve_time_s: 52
verified: true
draft: false
---

[CF 103637B - BSUIR Open X](https://codeforces.com/problemset/problem/103637/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi, mỗi chuỗi đại diện cho tên mã của một nhóm tác vụ. Từ bộ sưu tập này, chúng ta phải chọn chính xác hai chuỗi riêng biệt và chúng ta được phép đặt chúng theo một trong hai thứ tự, nối chuỗi này với chuỗi kia. 

Mục đích là đếm xem có bao nhiêu cặp chỉ số riêng biệt được sắp xếp tạo ra chuỗi mục tiêu “BSUIROPENX” sau khi nối. Tính khác biệt được xác định bằng chỉ số chứ không phải bằng giá trị chuỗi, vì vậy nếu hai chuỗi giống hệt nhau xuất hiện ở các vị trí khác nhau thì chúng được coi là các lựa chọn khác nhau. 

Ràng buộc chính là kích thước của đầu vào: tối đa 100.000 chuỗi với tổng chiều dài lên tới 1.000.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng kiểm tra trực tiếp tất cả các cặp chuỗi, vì đó sẽ là bậc hai về số lượng chuỗi và vượt xa giới hạn khả thi. Ngay cả kiểm tra O(n²) trong đó mỗi so sánh là O(1) sẽ có khoảng 10¹⁰ hoạt động trong trường hợp xấu nhất. 

Một ràng buộc tinh tế hơn đến từ độ dài chuỗi mục tiêu, cố định và nhỏ (10 ký tự). Điều đó gợi ý rằng chỉ những chuỗi có thể góp phần hình thành chuỗi chính xác này mới có liên quan và mọi thứ khác có thể bị bỏ qua. 

Một trường hợp thất bại phổ biến là giả sử chúng ta cần so khớp chuỗi con hoặc so khớp chồng chéo giữa các cặp tùy ý. Ví dụ: nếu một người cố gắng khớp các hậu tố và tiền tố một cách linh hoạt cho mỗi cặp, thì sẽ dễ dàng đếm quá mức hoặc bỏ sót các trường hợp trong đó điểm phân tách bị ràng buộc bởi đẳng thức chính xác thay vì khớp một phần. 

Một cạm bẫy cụ thể xuất hiện khi tồn tại nhiều chuỗi giống hệt nhau. Ví dụ: nếu danh sách chứa ba bản sao “BSU” và hai bản sao “IROPENX”, câu trả lời không phải là 3 + 2 mà là 3 × 2 cộng với các khả năng đảo ngược nếu có. Nhiều giải pháp sai quên rằng việc đặt hàng sẽ tăng gấp đôi mức đóng góp và các chuỗi giống hệt nhau vẫn phải được coi là các chỉ số riêng biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp chuỗi được sắp xếp và kiểm tra xem phép nối của chúng có bằng “BSUIROPENX” hay không. Mỗi séc có giá O(L) trong đó L tối đa là 10 và có các cặp O(n²). Với n lên tới 10⁵, điều này dẫn đến so sánh khoảng 10¹⁰, quá chậm. 

Quan sát quan trọng là chúng ta thực sự không cần phải xem xét các phép nối tùy ý. Vì chuỗi mục tiêu là cố định nên bất kỳ cặp hợp lệ nào cũng phải tương ứng với một điểm phân tách bên trong “BSUIROPENX”. Điều đó có nghĩa là chúng ta có thể chia mục tiêu thành hai phần và tìm kiếm một chuỗi bằng tiền tố và chuỗi khác bằng hậu tố. 

Đặt mục tiêu là T = “BSUIROPENX”. Đối với mỗi vị trí phân chia i từ 1 đến 9, chúng ta xem xét T[0:i] và T[i:]. Bất kỳ cặp thứ tự hợp lệ nào cũng phải bao gồm một chuỗi bằng T[0:i] và một chuỗi khác bằng T[i:]. Điều này làm giảm vấn đề đếm tần số của chuỗi. 

Chúng tôi chỉ cần đếm số lần mỗi chuỗi xuất hiện trong đầu vào, sau đó tính tổng tất cả các điểm phân chia hợp lệ bằng tích của tần số. Chúng tôi cũng phải tôn trọng thứ tự tự động: tiền tố thứ nhất và hậu tố thứ hai đã xác định hướng và chúng tôi tính toán riêng cho thứ tự đảo ngược nếu vấn đề yêu cầu cả hai thứ tự nối. Vì vậy, đối với mỗi phần tách, chúng tôi thêm cả freq(prefix) × freq(suffix) và freq(suffix) × freq(prefix) khi tiền tố và hậu tố khác nhau và chỉ freq(prefix) × (freq(prefix) − 1) khi chúng bằng nhau. 

Điều này làm giảm vấn đề đối với việc quét tuyến tính các chuỗi và làm việc liên tục trên mục tiêu có độ dài cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · L) | O(1) | Quá chậm | 
| Tần suất + liệt kê phân chia | O(n + | T | ) | 

## Hướng dẫn thuật toán 

Đặt chuỗi mục tiêu là T = “BSUIROPENX”.

1. Đọc tất cả các chuỗi đầu vào và xây dựng bản đồ tần số. Mỗi chuỗi riêng biệt lưu trữ số lần nó xuất hiện. Điều này cho phép tra cứu liên tục sau này thay vì quét lặp lại. 
2. Lặp lại tất cả các vị trí phân chia có thể có i từ 1 đến len(T) − 1. Mỗi phân tách xác định một tiền tố A = T[0:i] và hậu tố B = T[i:]. 
3. Đối với mỗi lần phân chia, hãy kiểm tra xem cả A và B có tồn tại trong bản đồ tần số hay không. Nếu thiếu một trong hai, hãy bỏ qua hoàn toàn phần phân tách này vì không thể tạo cặp hợp lệ. 
4. Nếu A và B là hai chuỗi khác nhau, hãy thêm freq[A] × freq[B] × 2 vào câu trả lời. Hệ số 2 chiếm cả hai đơn hàng có thể xảy ra: A theo sau là B và B theo sau là A. 
5. Nếu A và B là cùng một chuỗi, hãy thêm freq[A] × (freq[A] − 1). Điều này tính các cặp chỉ mục riêng biệt được sắp xếp được chọn từ các chuỗi giống hệt nhau, vì thứ tự vẫn quan trọng nhưng chúng ta phải tránh ghép một chỉ mục với chính nó. 
6. Tích lũy kết quả trên tất cả các điểm phân chia và xuất ra tổng cuối cùng. 

### Tại sao nó hoạt động 

Mỗi phép nối hợp lệ đều bằng chuỗi đích. Do đó, ranh giới giữa hai chuỗi được chọn phải căn chỉnh chính xác với một trong các vị trí cắt bên trong của mục tiêu. Không thể thực hiện được cấu trúc nào khác vì nối chuỗi sẽ duy trì trật tự mà không bị chồng chéo hoặc có khoảng trống. Điều này làm giảm toàn bộ không gian bài toán từ việc ghép chuỗi tùy ý thành một tập hữu hạn gồm tối đa 9 phần tách ứng viên. Việc đếm dựa trên tần số đảm bảo mọi cặp chỉ mục hợp lệ được tính chính xác một lần tùy theo liệu nó có tương ứng với sự phân chia tiền tố-hậu tố hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    freq = {}

    for _ in range(n):
        s = input().strip()
        freq[s] = freq.get(s, 0) + 1

    target = "BSUIROPENX"
    ans = 0

    m = len(target)

    for i in range(1, m):
        a = target[:i]
        b = target[i:]

        ca = freq.get(a, 0)
        cb = freq.get(b, 0)

        if ca == 0 or cb == 0:
            continue

        if a != b:
            ans += ca * cb * 2
        else:
            ans += ca * (ca - 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén đầu vào vào bảng tần số để các chuỗi lặp lại không cần phải quét nhiều lần. Sau đó, mỗi phần tách của chuỗi đích được coi như một giả thuyết cấu trúc về cách hình thành phép nối cuối cùng. Việc xử lý các nửa bằng nhau và không bằng nhau là rất quan trọng, vì nó xác định xem các cặp có thứ tự đến từ hai nhóm khác nhau hay trong một nhóm duy nhất. 

Phép nhân với 2 cho các phần riêng biệt rất dễ bị bỏ sót, nhưng nó trực tiếp tương ứng với thực tế là cả hai lệnh ghép đều hợp lệ và phải được tính riêng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
BSUIR
BSU
OPEN
IROPENX
```Mục tiêu là “BSUIROPENX”. Chúng tôi tính toán tần số: 

| chuỗi | tần số | 
| --- | --- | 
| BSUIR | 1 | 
| BSU | 1 | 
| MỞ | 1 | 
| IROPENX | 1 | 

Bây giờ chúng tôi kiểm tra sự phân chia: 

| chia tôi | tiền tố | hậu tố | tần số[tiền tố] | tần số[hậu tố] | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 3 | BSU | IROPENX | 1 | 1 | 2 | 

Các phần tách khác không khớp với bất kỳ chuỗi nào trong đầu vào. 

Đáp án cuối cùng là 2, tương ứng với cả hai thứ tự của (BSU, IROPENX). 

Điều này xác nhận rằng thứ tự được tính chính xác và chỉ có kết quả khớp tiền tố-hậu tố chính xác mới quan trọng. 

### Ví dụ 2 

đầu vào:```
3
BSUIR
OPENX
BSUIROPENX
```Sự phân chia mục tiêu bao gồm BSUIR + OPENX, nhưng OPENX hiện diện và BSUIR hiện diện. 

| chia tôi | tiền tố | hậu tố | tần số[tiền tố] | tần số[hậu tố] | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 5 | BSUIR | MỞ | 1 | 1 | 2 | 

Câu trả lời là 2, một lần nữa phản ánh cả hai thứ tự nối. 

Điều này chứng tỏ rằng ngay cả khi các chuỗi không liền kề nhau ở đầu vào, việc đếm tần số sẽ ghi lại chính xác tất cả các cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + | T | 
| Không gian | O(n) | Lưu trữ bản đồ tần số của chuỗi đầu vào | 

Các ràng buộc cho phép tối đa 10⁵ chuỗi và tổng chiều dài 10⁶, do đó, giải pháp thời gian tuyến tính với hàm băm nằm trong giới hạn. Chuỗi mục tiêu có kích thước không đổi, do đó việc liệt kê phân tách là không đáng kể. 

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

def solve():
    n = int(input().strip())
    freq = {}
    for _ in range(n):
        s = input().strip()
        freq[s] = freq.get(s, 0) + 1

    target = "BSUIROPENX"
    ans = 0
    m = len(target)

    for i in range(1, m):
        a = target[:i]
        b = target[i:]
        ca = freq.get(a, 0)
        cb = freq.get(b, 0)

        if ca == 0 or cb == 0:
            continue

        if a != b:
            ans += ca * cb * 2
        else:
            ans += ca * (ca - 1)

    print(ans)

# provided sample
assert run("4\nBSUIR\nBSU\nOPEN\nIROPENX\n") == "2"

# single valid pair
assert run("2\nBSUIR\nOPENX\n") == "2"

# no solution
assert run("3\nA\nB\nC\n") == "0"

# duplicates
assert run("4\nBSU\nBSU\nIROPENX\nIROPENX\n") == "8"

# all equal strings irrelevant
assert run("3\nBSU\nBSU\nBSU\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 2 | tính đúng đắn cơ bản | 
| 2 dây | 2 | cả hai đơn hàng đều được tính | 
| không khớp | 0 | từ chối an toàn | 
| trùng lặp | 8 | xử lý đa dạng | 
| lặp lại không liên quan | 0 | lọc các chuỗi không khớp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tiền tố và hậu tố giống hệt nhau. Nếu đầu vào chứa nhiều bản sao của một chuỗi như vậy, chúng ta phải đếm các cặp có thứ tự mà không ghép chỉ mục với chính nó. Công thức freq × (freq − 1) xử lý chính xác điều này. 

Ví dụ: nếu phân chia mục tiêu tạo ra A = B = "XX" và đầu vào chứa ba lần xuất hiện của "XX", thì các cặp có thứ tự hợp lệ là (i, j) với i ≠ j, cho 3 × 2 = 6. 

Một trường hợp cạnh khác là khi nhiều phần tách của mục tiêu tương ứng với các chuỗi hợp lệ. Mỗi phần chia đều độc lập nên các khoản đóng góp sẽ được tích lũy. Bản đồ tần số đảm bảo rằng mỗi cặp hợp lệ được tính chính xác một lần cho mỗi lần phân chia và không có sự trùng lặp giữa các lần phân tách tạo ra việc đếm gấp đôi ngoài tính đối xứng thứ tự dự kiến.
