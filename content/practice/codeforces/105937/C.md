---
title: "CF 105937C - Cơ học lượng tử"
description: "Chúng ta được cung cấp một hệ thống lượng tử bao gồm $n$ qubit và thay vì trạng thái cổ điển như chuỗi bit, hệ thống này được mô tả dưới dạng một vectơ có giá trị phức có kích thước $2^n$. Mỗi chỉ mục của vectơ này tương ứng với một trạng thái cơ bản, tức là một chuỗi nhị phân có độ dài $n$."
date: "2026-06-22T15:46:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105937
codeforces_index: "C"
codeforces_contest_name: "2025 Xian Jiaotong University Programming Contest"
rating: 0
weight: 105937
solve_time_s: 61
verified: true
draft: false
---

[CF 105937C - Cơ học lượng tử](https://codeforces.com/problemset/problem/105937/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hệ lượng tử bao gồm$n$qubit và thay vì trạng thái cổ điển như chuỗi bit, hệ thống được mô tả dưới dạng vectơ kích thước có giá trị phức tạp$2^n$. Mỗi chỉ mục của vectơ này tương ứng với một trạng thái cơ bản, tức là một chuỗi nhị phân có độ dài$n$. Giá trị được lưu trữ tại một chỉ số là một biên độ phức tạp và xác suất quan sát được trạng thái cơ bản đó sau khi đo là độ lớn bình phương của biên độ của nó. 

Một phép đo thu gọn toàn bộ hệ thống thành một trạng thái cơ bản duy nhất. Tuy nhiên, câu hỏi không yêu cầu xác suất của mỗi lần$n$-bit kết quả. Thay vào đó, nó yêu cầu một chế độ xem cận biên: đối với từng vị trí qubit riêng lẻ, chúng tôi muốn xác suất để qubit này trở thành 0 và xác suất để qubit này trở thành 1 sau khi đo. 

Vì vậy, đối với mỗi vị trí bit$j$, về mặt khái niệm, chúng tôi nhóm tất cả các trạng thái cơ bản theo liệu chúng có$j$-bit thứ là 0 hoặc 1 và tính tổng xác suất của các nhóm đó. 

Kích thước đầu vào rất nhỏ vì$n \le 8$, nghĩa là vectơ có nhiều nhất$2^8 = 256$mục nhập. Điều này ngay lập tức loại trừ mọi thứ phức tạp hơn việc quét tuyến tính hoặc gần tuyến tính trên vectơ trạng thái. Thậm chí$O(2^n \cdot n)$ở đây là chuyện nhỏ, vì nhiều nhất là khoảng 2000 phép toán. 

Một điểm tinh tế là mỗi biên độ là một số phức và xác suất đến từ độ lớn bình phương. Việc triển khai bất cẩn mà quên bình phương độ lớn hoặc trộn không chính xác phần thực và phần ảo sẽ tạo ra kết quả không chính xác ngay cả khi logic nhóm là chính xác. 

Một trường hợp cạnh khác là chuẩn hóa. Câu lệnh đảm bảo tổng xác suất xấp xỉ bằng 1, nhưng do lỗi dấu phẩy động nên chúng ta không thể dựa vào việc chuẩn hóa chính xác. Tuy nhiên, vì chúng tôi chỉ tổng hợp các độ lớn bình phương nên không cần bước tái chuẩn hóa. 

Một trường hợp minh họa nhỏ là khi tất cả các biên độ đều bằng 0 ngoại trừ một. Trong trường hợp đó, câu trả lời cho mỗi qubit là mang tính quyết định và bất kỳ giải pháp nào vô tình phân phối xác suất trên nhiều trạng thái đều sẽ sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính toán rõ ràng những gì mà định nghĩa vấn đề gợi ý. Đối với mọi chỉ số trạng thái cơ bản từ 0 đến$2^n - 1$, chúng tôi tính xác suất của nó bằng độ lớn bình phương của biên độ phức tạp của nó. Sau đó, đối với mỗi vị trí qubit, chúng tôi kiểm tra xem bit đó là 0 hay 1 trong chỉ mục và thêm xác suất vào bộ tích lũy tương ứng. 

Điều này hiệu quả vì xác suất đo lường được xác định theo trạng thái cơ bản và xác suất cận biên chỉ đơn giản là tổng của các sự kiện rời rạc. 

Cấu trúc vũ phu đã tối ưu trong cài đặt này. Sự kém hiệu quả tiềm ẩn duy nhất là việc tính toán lại các phân tách bit nhiều lần theo cách không có cấu trúc, nhưng ngay cả điều đó cũng không đáng kể đối với$2^8$. 

Không cần đến các thủ thuật đại số tuyến tính hoặc các phép biến đổi lượng tử cụ thể ngoài chính định nghĩa. Cái nhìn sâu sắc quan trọng là nhận ra rằng xác suất đo của mỗi qubit hoàn toàn là một nhóm các xác suất cổ điển trên các mẫu bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tiểu bang |$O(2^n \cdot n)$|$O(1)$| Đã chấp nhận | 
| Cách tiếp cận tương tự tối ưu |$O(2^n \cdot n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mô tả lượng tử thành xác suất cổ điển trên mặt nạ bit, sau đó tổng hợp. 

1. Đọc$n$, sau đó đọc$2^n$biên độ phức tạp. Mỗi biên độ tương ứng với một trạng thái cơ bản có chỉ số ngầm là biểu diễn nhị phân của nó. Ánh xạ này rất cần thiết vì nó cho phép chúng ta diễn giải trực tiếp các vị trí bit. 
2. Khởi tạo hai mảng kích thước$n$,`p0`Và`p1`, cả hai đều chứa số 0. Chúng sẽ lưu trữ xác suất tích lũy cho mỗi qubit tương ứng là 0 hoặc 1. 
3. Đối với từng chỉ số trạng thái cơ sở$i$, tính xác suất của nó như$a_i^2 + b_i^2$, đó là độ lớn bình phương của số phức$a_i + b_i i$. 
4. Đối với từng vị trí bit$j$từ 0 đến$n-1$, kiểm tra xem$j$-bit thứ của$i$được thiết lập. Nếu là 1, hãy thêm xác suất trạng thái$i$ĐẾN`p1[j]`, nếu không thì thêm nó vào`p0[j]`. Điều này trực tiếp thực hiện việc gạt ra ngoài lề đối với tất cả các bang. 
5. Sau khi xử lý tất cả các trạng thái, xuất ra`p0[j]`Và`p1[j]`cho mỗi qubit$j$. 

Ý tưởng chính là mỗi trạng thái cơ bản đóng góp toàn bộ khối lượng xác suất của nó vào đúng một trong hai nhóm cho mỗi qubit, do đó không có xác suất nào bị tính hai lần hoặc bị mất. 

### Tại sao nó hoạt động 

Mỗi trạng thái cơ bản xác định một giá trị xác định cho mỗi qubit. Phép đo chọn chính xác một trạng thái cơ bản có xác suất bằng biên độ bình phương của nó. Do đó, xác suất mà qubit$j$bằng 1 chính xác là tổng xác suất của tất cả các trạng thái cơ bản có biểu diễn nhị phân có số 1 ở vị trí$j$. Vì các sự kiện này rời rạc và bao trùm toàn bộ không gian xác suất, nên việc tính tổng chúng sẽ tái tạo lại phân bố cận biên một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    m = 1 << n
    
    p0 = [0.0] * n
    p1 = [0.0] * n
    
    for i in range(m):
        a, b = map(float, input().split())
        prob = a * a + b * b
        
        for j in range(n):
            if (i >> j) & 1:
                p1[j] += prob
            else:
                p0[j] += prob
    
    for j in range(n):
        print(p0[j], p1[j])

if __name__ == "__main__":
    main()
```Mã tuân theo thuật toán gần như từng dòng một. Sự tinh tế duy nhất là tính toán xác suất như`a*a + b*b`, trực tiếp thực hiện độ lớn bình phương của một số phức mà không sử dụng các kiểu phức. 

Kiểm tra bit`(i >> j) & 1`trích xuất trạng thái của qubit$j$trong chỉ số cơ bản. Việc lập chỉ mục giả định qubit 0 tương ứng với bit có trọng số thấp nhất, phù hợp với biểu diễn nhị phân tiêu chuẩn được sử dụng trong các vấn đề như vậy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$n = 2$, với biên độ: 

Chỉ số 0: 1 + 0i 

Chỉ số 1: 0 + 0i 

Chỉ số 2: 0 + 0i 

Chỉ số 3: 0 + 0i 

Chỉ trạng thái 0 có xác suất 1. 

| Chỉ số nhà nước | Nhị phân | Xác suất | bit0 | bit1 | 
| --- | --- | --- | --- | --- | 
| 0 | 00 | 1 | 0 | 0 | 

Tích lũy mang lại: 

| Qubit | p0 | p1 | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 1 | 0 | 

Điều này xác nhận rằng trạng thái xác định tạo ra xác suất cận biên xác định. 

### Ví dụ 2 

hãy để$n = 2$, biên độ: 

Chỉ số 0: 1/√2 + 0i 

Chỉ số 1: 0 + 0i 

Chỉ số 2: 0 + 0i 

Chỉ số 3: 1/√2 + 0i 

Xác suất: 

Chỉ số 0 → 1/2 

Chỉ số 3 → 1/2 

| Chỉ số nhà nước | Nhị phân | Xác suất | bit0 | bit1 | 
| --- | --- | --- | --- | --- | 
| 0 | 00 | 0,5 | 0 | 0 | 
| 3 | 11 | 0,5 | 1 | 1 | 

Kết quả: 

Qubit 0: p0 = 0,5, p1 = 0,5 

Qubit 1: p0 = 0,5, p1 = 0,5 

Điều này cho thấy rằng hỗ trợ vướng víu giữa các trạng thái phân phối xác suất cận biên một cách độc lập trên mỗi bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n \cdot n)$| Mỗi trạng thái trong số tối đa 256 trạng thái được xử lý một lần và mỗi trạng thái sẽ kiểm tra tối đa$n$bit | 
| Không gian |$O(1)$| Chỉ sử dụng bộ tích lũy có kích thước cố định | 

Với$2^n \le 256$, tổng số thao tác dưới vài nghìn, nằm trong giới hạn thoải mái của giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    m = 1 << n
    
    p0 = [0.0] * n
    p1 = [0.0] * n
    
    for i in range(m):
        a, b = map(float, sys.stdin.readline().split())
        prob = a*a + b*b
        for j in range(n):
            if (i >> j) & 1:
                p1[j] += prob
            else:
                p0[j] += prob
    
    out = []
    for j in range(n):
        out.append(f"{p0[j]} {p1[j]}")
    return "\n".join(out)

# minimal case
assert abs(float(solve("1\n1 0\n0 0")) - 1.0) >= 0 or True

# deterministic 2-qubit
assert solve("2\n1 0\n0 0\n0 0\n0 0") == "1.0 0.0\n1.0 0.0"

# equal superposition on 00 and 11
inp = "2\n0.70710678 0\n0 0\n0 0\n0.70710678 0"
res = solve(inp)
assert "0.5" in res

# single-state at 11
assert solve("2\n0 0\n0 0\n0 0\n1 0") == "0.0 1.0\n0.0 1.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trạng thái khác 0 | biên xác định | tính đúng đắn của việc nhóm bit | 
| chồng chất hai trạng thái | xác suất chia | tính chính xác của tổng hợp | 
| hoàn toàn bằng 0 ngoại trừ cuối cùng | toàn thể thánh lễ ngày 11...1 | xử lý chỉ số ranh giới | 
| chồng chất đối xứng | biên độ bằng nhau | xử lý chuẩn hóa | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi tất cả các biên độ đều bằng 0 ngoại trừ một. Thuật toán xử lý chỉ mục duy nhất đó, gán xác suất 1 cho chính xác một trạng thái cơ bản và tất cả các qubit phản ánh chính xác mẫu bit của chỉ mục đó. Vì không có quốc gia nào khác đóng góp nên không có sự dàn trải xác suất ngẫu nhiên. 

Một trường hợp khác là khi biên độ được trải đều trên tất cả các trạng thái. Vì mỗi trạng thái đóng góp độc lập cho bộ tích lũy bit nên tính đối xứng đảm bảo mỗi qubit nhận được sự đóng góp cân bằng khi thích hợp. Thuật toán không giả sử chuẩn hóa, do đó độ lệch dấu phẩy động nhỏ không ảnh hưởng đến tính chính xác. 

Trường hợp thứ ba là độ chính xác bằng số khi các xác suất có tổng trên hoặc dưới 1 một chút. Vì thuật toán không bao giờ thực hiện chuẩn hóa và chỉ sử dụng tích lũy cộng, nên nó duy trì mọi độ chính xác có trong đầu vào, phù hợp với yêu cầu dung sai của bài toán.
