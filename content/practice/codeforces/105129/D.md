---
title: "CF 105129D - Hai Hình Tam Giác"
description: "Mỗi trường hợp thử nghiệm mô tả một tam giác vuông bằng cách sử dụng đáy và chiều cao của nó. Nhiệm vụ không phải là tính diện tích hình học thực tế mà thay vào đó là xuất ra diện tích gấp đôi diện tích đó. Diện tích của một tam giác vuông được tính bằng một nửa tích của đáy và chiều cao của nó."
date: "2026-06-27T18:53:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "D"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 38
verified: true
draft: false
---

[CF 105129D - Hai hình tam giác](https://codeforces.com/problemset/problem/105129/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một tam giác vuông bằng cách sử dụng đáy và chiều cao của nó. Nhiệm vụ không phải là tính diện tích hình học thực tế mà thay vào đó là xuất ra diện tích gấp đôi diện tích đó. 

Diện tích của một tam giác vuông được tính bằng một nửa tích của đáy và chiều cao của nó. Vì bài toán yêu cầu diện tích gấp đôi nên phân số biến mất và mỗi trường hợp kiểm thử rút gọn thành một phép nhân đơn giản của hai số nguyên đã cho. 

Đầu vào bắt đầu bằng một số cho biết chúng tôi sẽ xử lý bao nhiêu hình tam giác. Sau đó, mỗi dòng tiếp theo cung cấp đáy và chiều cao của một hình tam giác. Với mỗi cặp, chúng ta phải xuất ra một số nguyên duy nhất biểu thị$b \times h$. 

Các ràng buộc nhỏ, với cả hai chiều lên tới 500. Ngay cả khi có nhiều trường hợp thử nghiệm, phép toán cho mỗi trường hợp là số học theo thời gian không đổi trên các số nguyên nhỏ. Điều này có nghĩa là mọi giải pháp đúng đều có thể chạy thoải mái trong giới hạn miễn là tránh được chi phí không cần thiết. 

Kiểu lỗi chính trong các vấn đề tương tự thường xuất phát từ việc đọc sai định dạng đầu vào hoặc quên rằng số lượng được yêu cầu gấp đôi diện tích chứ không phải chính diện tích. Một vấn đề tinh tế khác là coi phép tính là phép chia dấu phẩy động cho hai rồi nhân lại, điều này là không cần thiết và có thể đưa ra cách xử lý chính xác không cần thiết ở đây. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề là áp dụng định nghĩa diện tích tam giác theo đúng nghĩa đen cho từng trường hợp thử nghiệm. Với một đáy và chiều cao cho trước, hãy tính$\frac{b \cdot h}{2}$, sau đó nhân với 2 theo yêu cầu. Điều này ngay lập tức đơn giản hóa thành$b \cdot h$, do đó bước chia trung gian không có tác dụng gì. 

Việc triển khai bạo lực vẫn có thể tuân theo chính xác công thức: tính tích, chia cho hai, sau đó nhân với hai. Điều này đúng về mặt toán học, nhưng nó gây ra các phép toán dư thừa và khả năng sử dụng dấu phẩy động nếu thực hiện bất cẩn. Chi phí tính toán vẫn không đáng kể vì mỗi trường hợp thử nghiệm chỉ yêu cầu một vài phép tính số học, do đó hiệu suất không bao giờ là yếu tố hạn chế. 

Quan sát quan trọng là bài toán được thiết kế để kiểm tra xem người giải có nhận ra sự đơn giản hóa đại số hay không. Cấu trúc của công thức loại bỏ phép toán không cần thiết duy nhất, để lại phép nhân trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh giá công thức nghĩa đen | O(T) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Đơn giản hóa trực tiếp | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$T$, xác định số lượng phép tính độc lập phải được thực hiện. 
2. Với mỗi test, đọc hai số nguyên$b$Và$h$, biểu thị kích thước của một tam giác vuông. 
3. Tính tích$b \times h$, tương ứng với hai lần diện tích của tam giác sau khi đơn giản hóa. 
4. Xuất ngay giá trị tính toán cho từng test. 

### Tại sao nó hoạt động 

Diện tích của một tam giác vuông được xác định là$\frac{b \cdot h}{2}$. Bài toán yêu cầu gấp đôi giá trị này nên hệ số của$\frac{1}{2}$hủy bỏ chính xác bằng phép nhân với 2. Điều này để lại sự bằng nhau trực tiếp giữa đầu ra cần thiết và sản phẩm$b \cdot h$. Vì mỗi trường hợp kiểm thử là độc lập và chỉ liên quan đến số học theo thời gian không đổi nên việc xử lý chúng một cách tuần tự sẽ duy trì tính chính xác mà không yêu cầu bất kỳ trạng thái hoặc tiền xử lý bổ sung nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        b, h = map(int, input().split())
        out.append(str(b * h))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp thử nghiệm một cách tuần tự và lưu trữ kết quả vào danh sách trước khi in chúng cùng một lúc. Điều này tránh các lệnh gọi I/O lặp lại, đây là một cách tối ưu hóa tiêu chuẩn trong lập trình cạnh tranh Python. 

Phép tính duy nhất bên trong vòng lặp là phép nhân một số nguyên. Không sử dụng số học dấu phẩy động, điều này tránh hoàn toàn các vấn đề về độ chính xác. Việc sử dụng đầu ra được đệm đảm bảo rằng ngay cả các giá trị lớn của$T$được xử lý một cách hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có hai hình tam giác: 

đầu vào:```
2
4 8
10 12
```Đối với mỗi trường hợp thử nghiệm, chúng tôi theo dõi tính toán. 

| Trường hợp thử nghiệm | b | h | b×h | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 8 | 32 | 32 | 
| 2 | 10 | 12 | 120 | 120 | 

Tam giác đầu tiên tạo ra 32 và tam giác thứ hai tạo ra 120. Mỗi đầu ra độc lập, xác nhận rằng không có trạng thái chia sẻ nào liên quan đến các trường hợp thử nghiệm. 

Dấu vết này cho thấy thuật toán biến đổi trực tiếp từng cặp đầu vào thành sản phẩm của nó mà không qua các bước trung gian, khớp với dạng toán học đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu một phép nhân và phân tích cú pháp theo thời gian không đổi | 
| Không gian | O(1) | Ngoài bộ lưu trữ đầu ra, không có cấu trúc dữ liệu phụ trợ nào được sử dụng | 

Các ràng buộc cho phép tối đa số lượng trường hợp thử nghiệm vừa phải với các giá trị số nguyên nhỏ, do đó việc quét tuyến tính qua đầu vào là đủ dễ dàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        b, h = map(int, input().split())
        out.append(str(b * h))
    return "\n".join(out)

# provided sample
assert run("2\n4 8\n10 12\n") == "32\n120"

# minimum input
assert run("1\n1 1\n") == "1"

# all equal values
assert run("3\n5 5\n5 5\n5 5\n") == "25\n25\n25"

# boundary values
assert run("2\n500 1\n500 500\n") == "500\n250000"

# mixed pattern
assert run("4\n2 3\n3 4\n4 5\n5 6\n") == "6\n12\n20\n30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp tối thiểu duy nhất | 1 | trường hợp hợp lệ nhỏ nhất | 
| cặp giống hệt nhau lặp đi lặp lại | lặp lại liên tục | tính nhất quán giữa các trường hợp | 
| trường hợp cơ bản tối đa | sản phẩm lớn | độ đúng số học biên | 
| mô hình tăng dần | tính đúng đắn tuần tự | độ chính xác chung trên nhiều đầu vào | 

## Vỏ cạnh 

Tam giác nhỏ nhất có thể xảy ra khi cả cạnh đáy và chiều cao đều bằng 1. Thuật toán đọc cặp, nhân chúng và trả về 1. Không cần xử lý đặc biệt vì phép nhân hoạt động chính xác ở tỷ lệ này. 

Ở giới hạn trên, cả hai giá trị có thể là 500, tạo ra 250000. Điều này vẫn vừa vặn thoải mái trong phạm vi số nguyên 32 bit tiêu chuẩn, do đó không có vấn đề tràn phát sinh trong Python. Thuật toán chỉ đơn giản là tính toán sản phẩm và xuất ra trực tiếp. 

Khi tất cả các trường hợp thử nghiệm đều giống hệt nhau, thuật toán vẫn xử lý từng trường hợp độc lập mà không dựa vào các giá trị được lưu trong bộ nhớ đệm. Mỗi phép nhân được tính toán lại, giúp duy trì tính chính xác mặc dù việc tối ưu hóa là không cần thiết ở quy mô này.
