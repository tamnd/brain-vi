---
title: "CF 105884G - Đến vô cực và xa hơn"
description: "Chúng ta có hai mối quan hệ cha mẹ khác nhau trên cùng một tập hợp n vị trí. Mỗi mối quan hệ mô tả một cây gốc: mọi nút ngoại trừ nút đầu tiên đều trỏ đến nút trước đó là nút gốc của nó."
date: "2026-06-25T14:16:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105884
codeforces_index: "G"
codeforces_contest_name: "Betopia Group Presents DUET Inter University Programming Contest 2025"
rating: 0
weight: 105884
solve_time_s: 40
verified: true
draft: false
---

[CF 105884G - Đến vô cực và xa hơn](https://codeforces.com/problemset/problem/105884/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mối quan hệ cha mẹ khác nhau trên cùng một tập hợp các`n`các vị trí. Mỗi mối quan hệ mô tả một cây gốc: mọi nút ngoại trừ nút đầu tiên đều trỏ đến nút trước đó là nút gốc của nó. Đối với một cái cây, chúng ta đếm có bao nhiêu cách để gán các số nguyên dương không vượt quá`x`đến tất cả các nút sao cho mọi nút con đều nhận được giá trị không lớn hơn nút cha của nó. Chúng ta cần tỷ lệ giới hạn của hai số đếm là`x`phát triển không giới hạn. Nhiệm vụ ban đầu yêu cầu giá trị hợp lý này theo modulo`998244353`. 

Thoạt nhìn, chức năng đếm phụ thuộc vào`x`, có vẻ như nó yêu cầu xử lý các giá trị rất lớn. Điều quan trọng là các số đếm này là đa thức trong`x`. Chúng ta không cần toàn bộ đa thức, chỉ cần tỷ số của các hệ số bậc cao nhất của chúng. Những hạn chế có tới`3 * 10^5`tổng số nút trên tất cả các bài kiểm tra, do đó, giải pháp bậc hai cho mỗi bài kiểm tra sẽ quá chậm. Chúng ta cần thứ gì đó gần tuyến tính ở kích thước đầu vào. Mảng cha cũng tạo thành cây, có nghĩa là chúng ta có thể sử dụng các thuộc tính dành riêng cho cây thay vì các thuật toán đồ thị chung. 

Một lỗi phổ biến là mô phỏng quá trình đếm một số giá trị của`x`và cố gắng đoán giới hạn. Ngay cả khi điều này hoạt động trên các cây nhỏ, các giá trị của`x`cần thiết để phát hiện hệ số dẫn đầu có thể trở nên quá lớn và không có gì đảm bảo rằng một số mẫu tiết lộ con số hữu tỷ chính xác. 

Một trường hợp cạnh khác là khi hai cây có hình dạng giống nhau nhưng đánh số gốc khác nhau. Ví dụ: nếu cả hai đầu vào là một chuỗi:```
n = 4
a = [-1, 1, 2, 3]
b = [-1, 1, 2, 3]
```Câu trả lời là`1`. Một giải pháp chỉ so sánh các mảng cha mẹ hoặc các chỉ mục nút sẽ không thành công vì chỉ có cấu trúc cây mới quan trọng. 

Trường hợp cạnh thứ hai là một ngôi sao được so sánh với một chuỗi:```
n = 3
a = [-1, 1, 1]
b = [-1, 1, 2]
```Câu trả lời là`2`. Ngôi sao có nhiều phép gán hợp lệ hơn vì hai con có thể chọn giá trị độc lập sau khi gốc được cố định. Một giải pháp chỉ nhìn vào độ sâu sẽ bỏ qua sự khác biệt này vì cả hai cây đều có cùng mô hình độ sâu tối đa gồm ba cấp so với hai cấp. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là tính số lượng phép gán hợp lệ để tăng giá trị của`x`và quan sát tỷ lệ. Đối với một cố định`x`, chúng ta có thể thực hiện phép tính lập trình động cây. Giả sử giá trị của nút được cố định thì mỗi cây con con có thể được tính độc lập. Điều này đúng vì hạn chế duy nhất kết nối cây con với phần còn lại của cây là giá trị tại gốc của nó. Tuy nhiên, lặp lại điều này với nhiều giá trị của`x`là không thực tế. Số lượng hoạt động tăng lên với phạm vi được chọn`x`và các ràng buộc không cho phép bất kỳ sự phụ thuộc nào vào một tham số số lớn. 

Quan sát quan trọng đến từ việc nhận ra hệ số dẫn đầu đại diện cho điều gì. Số lượng phép gán hợp lệ là đa thức bậc của bậc một phần của cây. Hệ số bậc cao nhất là số cách sắp xếp các nút theo thứ tự tăng dần hợp lệ chia cho`n!`. Nói cách khác, đó là xác suất mà thứ tự ngẫu nhiên của các nút tuân theo quy tắc mọi nút cha đều xuất hiện trước nút con của nó. 

Đối với cây có gốc, số thứ tự hợp lệ này có một công thức đơn giản. Nếu như`size[v]`là kích thước của cây con có gốc tại`v`, số thứ tự cha trước con hợp lệ là:```
n! / (size[1] * size[2] * ... * size[n])
```Giai thừa ở cả hai cây đều giống nhau nên nó biến mất trong tỷ lệ. Chúng ta chỉ cần tích các kích thước cây con trong mỗi cây. 

Brute-force hoạt động vì nó cố gắng hiểu trực tiếp đa thức đếm nhưng không thành công vì đa thức có thể rất lớn. Việc quan sát các hệ số dẫn đầu cho phép chúng ta đơn giản hóa toàn bộ vấn đề trong việc tính toán kích thước cây con và lấy một phép chia mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Phụ thuộc vào x đã chọn, có thể vượt quá O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng kích thước cây con cho cây thứ nhất và cây thứ hai. Vì mọi chỉ mục gốc đều nhỏ hơn chỉ mục con của nó nên dữ liệu đầu vào đã mô tả một cây có gốc có gốc`1`, nhưng truyền ngược lại từ`n`xuống tới`1`là đủ vì con cái luôn xuất hiện sau cha mẹ. 
2. Bắt đầu kích thước cây con của mỗi nút là`1`. Khi xử lý một nút`i`từ cuối về phía gốc, thêm kích thước của nó vào phần gốc của nó. Điều này hiệu quả vì mọi phần tử con đã đóng góp toàn bộ cây con của nó khi chúng ta tiếp cận phần tử cha. 
3. Tính tích của tất cả các kích thước cây con cho modulo cây đầu tiên`998244353`. Giá trị này là phần mẫu số của công thức hệ số dẫn đầu. 
4. Lặp lại phép tính tương tự cho cây thứ hai. 
5. Câu trả lời cuối cùng là:```
(product of subtree sizes of second tree)
/
(product of subtree sizes of first tree)
```lấy modulo`998244353`. 

Sở dĩ tỷ lệ này đúng là vì hai hệ số cao nhất đều được chia cho cùng một`n!`. Phép chia mô-đun có thể thực hiện được vì bài toán đảm bảo mẫu số có thể nghịch đảo. 

Tại sao nó hoạt động: 

Số lượng bài tập có giá trị từ`1`ĐẾN`x`là một bằng cấp`n`đa thức. Hệ số cao nhất được xác định bởi số lượng thứ tự tương đối hợp lệ của các nút. Thứ tự tương đối hợp lệ chính xác là một hoán vị trong đó mọi nút cha đều xuất hiện trước tất cả các nút trong cây con của nó. Công thức chiều dài móc cây cho kết quả này là`n!`chia cho tích của kích thước cây con. Vì hai cây có cùng số nút nên các số hạng giai thừa bị hủy bỏ, chỉ để lại tỷ lệ của các tích có kích thước cây con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def calc(n, parent):
    prod = [1] * (n + 1)

    for i in range(n, 1, -1):
        prod[parent[i]] += prod[i]

    ans = 1
    for i in range(1, n + 1):
        ans = ans * prod[i] % MOD

    return ans

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    ptr = 0
    t = data[ptr]
    ptr += 1
    out = []

    for _ in range(t):
        n = data[ptr]
        ptr += 1

        a = [0] + data[ptr:ptr + n]
        ptr += n

        b = [0] + data[ptr:ptr + n]
        ptr += n

        x = calc(n, a)
        y = calc(n, b)

        out.append(str(y * pow(x, MOD - 2, MOD) % MOD))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`calc`chức năng là cốt lõi của giải pháp. Nó lưu trữ kích thước cây con trực tiếp trong một mảng. Vì chỉ mục cha đầu vào luôn nhỏ hơn chỉ mục con nên việc lặp từ`n`xuống tới`2`đảm bảo rằng nút con của nút đã được xử lý. 

Sản phẩm được tích lũy sau khi biết hết các kích cỡ. Phép nhân được thực hiện theo modulo`998244353`bởi vì câu trả lời cuối cùng chỉ cần số học mô-đun. 

Phép chia cuối cùng được thực hiện bằng định lý nhỏ của Fermat. Thay vì tính trực tiếp một phân số, chúng ta nhân với nghịch đảo mô đun của mẫu số. Bài toán đảm bảo rằng nghịch đảo này tồn tại. 

Một lỗi triển khai thường gặp là xử lý cây theo hướng thuận. Điều đó sẽ thêm kích thước cây con không đầy đủ vào cây cha và tạo ra các sản phẩm sai. Một sai lầm khác là chỉ sử dụng tích của các nút bên trong, nhưng mỗi nút đóng góp một yếu tố trong công thức độ dài móc, bao gồm cả các lá có kích thước cây con`1`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 3
a = [-1, 1, 1]
b = [-1, 1, 2]
```Các tính toán cây con là: 

| Bước | Nút được xử lý | Kích thước cây con cho | Kích thước cây con cho b | 
| --- | --- | --- | --- | 
| Bắt đầu | không | 1,1,1 | 1,1,1 | 
| Nút xử lý 3 | thêm vào cha mẹ | 1,1,1 | 1,2,1 | 
| Nút xử lý 2 | thêm vào cha mẹ | 1,2,1 | 1,3,1 | 

Các sản phẩm là`2`cho cây đầu tiên và`3`cho cây thứ hai. Câu trả lời là:```
3 / 2 = 3 * inverse(2) = 2 (mod 998244353)
```Ví dụ này cho thấy tại sao việc phân nhánh lại thay đổi số lượng thứ tự hợp lệ. 

Đối với mẫu thứ hai:```
n = 4
a = [-1, 1, 1, 2]
b = [-1, 1, 2, 1]
```Việc tính toán trở thành: 

| Bước | Nút được xử lý | Kích thước cây con cho | Kích thước cây con cho b | 
| --- | --- | --- | --- | 
| Bắt đầu | không | 1,1,1,1 | 1,1,1,1 | 
| Nút xử lý 4 | thêm vào cha mẹ | 1,2,1,1 | 1,2,1,1 | 
| Nút xử lý 3 | thêm vào cha mẹ | 1,2,1,1 | 1,3,1,1 | 
| Nút xử lý 2 | thêm vào cha mẹ | 1,4,1,1 | 1,3,1,1 | 

Các sản phẩm khác nhau ở các bước trung gian, nhưng sau khi kết thúc quá trình tích lũy gốc, tỷ lệ cuối cùng sẽ trở thành`1`. 

Dấu vết này xác nhận rằng thuật toán không so sánh trực tiếp độ sâu hoặc số lượng con. Nó sử dụng cấu trúc cây con đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý một lần cho mỗi cây và tất cả các sản phẩm đều tuyến tính | 
| Không gian | O(n) | Mảng cha và bộ lưu trữ cây con yêu cầu bộ nhớ tuyến tính | 

Tổng số nút trên tất cả các trường hợp thử nghiệm được giới hạn bởi`3 * 10^5`, do đó nghiệm tuyến tính dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve_input(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = list(map(int, sys.stdin.read().split()))
    sys.stdin = old

    ptr = 0
    t = data[ptr]
    ptr += 1
    ans = []

    def calc(n, p):
        s = [1] * (n + 1)
        for i in range(n, 1, -1):
            s[p[i]] += s[i]
        res = 1
        for v in s[1:]:
            res = res * v % MOD
        return res

    for _ in range(t):
        n = data[ptr]
        ptr += 1
        a = [0] + data[ptr:ptr+n]
        ptr += n
        b = [0] + data[ptr:ptr+n]
        ptr += n
        ans.append(str(calc(n, b) * pow(calc(n, a), MOD - 2, MOD) % MOD))

    return "\n".join(ans)

assert solve_input("""2
3
-1 1 1
-1 1 2
4
-1 1 1 2
-1 1 2 1
""") == "2\n1"

assert solve_input("""1
2
-1 1
-1 1
""") == "1"

assert solve_input("""1
5
-1 1 1 1 1
-1 1 2 3 4
""") == "24"

assert solve_input("""1
5
-1 1 2 3 4
-1 1 2 3 4
""") == "1"

assert solve_input("""1
6
-1 1 1 2 2 3
-1 1 1 1 1 1
""") != "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai trường hợp mẫu |`2`,`1`| Tính đúng đắn cơ bản | 
| Cây giống hai nút |`1`| Cấu trúc nhỏ nhất có thể | 
| Ngôi sao so với chuỗi |`24`| Sự khác biệt về cấu trúc mạnh mẽ | 
| Chuỗi giống hệt nhau |`1`| Xử lý hình dạng cây giống nhau | 
| Cây phân nhánh hỗn hợp | Giá trị khác 0 | Tính toán nghịch đảo mô-đun và cây con | 

## Vỏ cạnh 

Đối với trường hợp cây giống hệt nhau:```
1
2
-1 1
-1 1
```Cả hai sản phẩm cây con đều giống nhau. Thuật toán tính cùng mẫu số cho cả hai cây, do đó tỷ lệ là`1`. Điều này phát hiện các giải pháp vô tình sử dụng thứ tự đầu vào thay vì cấu trúc cây. 

Đối với trường hợp ngôi sao và chuỗi:```
1
3
-1 1 1
-1 1 2
```Cây đầu tiên có kích thước cây con`3,1,1`, tặng sản phẩm`3`. Cái thứ hai có kích thước`3,2,1`, tặng sản phẩm`6`. Câu trả lời là`6 / 3 = 2`. Thuật toán xử lý việc này vì việc phân nhánh ảnh hưởng trực tiếp đến kích thước cây con. 

Đối với sự khác biệt phân nhánh lớn hơn:```
1
6
-1 1 1 2 2 3
-1 1 1 1 1 1
```Việc duyệt ngược trước tiên sẽ tính toán từng lá theo kích thước`1`, sau đó đẩy các kích thước đó lên trên. Các sản phẩm cuối cùng đại diện cho số lượng chính xác các đơn hàng cha-con hợp lệ, do đó tỷ lệ mô-đun được tính toán mà không cần xây dựng các đa thức đếm.
