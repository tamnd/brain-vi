---
title: "CF 1863B - Sắp xếp phân chia"
description: "Chúng tôi được cấp một hoán vị các số từ 1 đến n và chúng tôi muốn chuyển đổi nó thành hoán vị nhận dạng trong đó mỗi giá trị nằm ở vị trí chỉ mục phù hợp của nó."
date: "2026-06-09T00:01:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1863
codeforces_index: "B"
codeforces_contest_name: "Pinely Round 2 (Div. 1 + Div. 2)"
rating: 1100
weight: 1863
solve_time_s: 96
verified: false
draft: false
---

[CF 1863B - Sắp xếp phân tách](https://codeforces.com/problemset/problem/1863/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hoán vị các số từ 1 đến n và chúng tôi muốn chuyển đổi nó thành hoán vị nhận dạng trong đó mỗi giá trị nằm ở vị trí chỉ mục phù hợp của nó. Động thái duy nhất được phép là một loại phân vùng ổn định: chọn một giá trị ngưỡng x, sau đó sắp xếp lại mảng bằng cách lấy tất cả các phần tử nhỏ hơn x theo thứ tự tương đối hiện tại của chúng, theo sau là tất cả các phần tử lớn hơn hoặc bằng x, một lần nữa giữ nguyên thứ tự tương đối của chúng. 

Hoạt động này không cho phép sắp xếp lại tùy ý. Nó chỉ cho phép chúng tôi phân chia chuỗi nhiều lần dựa trên ngưỡng giá trị trong khi vẫn duy trì trật tự nội bộ trong mỗi nhóm. Mục tiêu là xác định số lượng phân chia tối thiểu cần thiết để sắp xếp đầy đủ hoán vị. 

Ràng buộc n lên tới 100.000 với tổng số tiền cũng lên tới 100.000 trong các thử nghiệm ngụ ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai, chẳng hạn như mô phỏng từng thao tác một cách rõ ràng hoặc liên tục xây dựng lại các mảng, sẽ không tồn tại. 

Một khía cạnh tinh tế là hoạt động phụ thuộc vào giá trị, không phải vị trí, nhưng ảnh hưởng đến vị trí trên toàn cầu. Một trực giác ngây thơ có thể cho rằng chúng ta đang “sắp xếp dần dần”, nhưng hạn chế là mỗi thao tác là một phân vùng duy nhất khiến quá trình này có cấu trúc chặt chẽ hơn so với việc sắp xếp thông thường. 

Các trường hợp biên thường gây nhầm lẫn bao gồm các mảng đã được sắp xếp, các mảng bị đảo ngược hoàn toàn và các hoán vị trong đó thứ tự cục bộ trông có vẻ được sắp xếp một phần nhưng cấu trúc toàn cục vẫn yêu cầu nhiều phân vùng. Ví dụ, một hoán vị như`[2, 1, 3, 4]`gần như đã được sắp xếp nhưng vẫn chỉ cần một thao tác vì việc phân chia ngưỡng duy nhất có thể tách biệt chính xác 1 và 2. 

Một cái bẫy khác là giả định rằng chúng ta có thể sửa một phần tử cho mỗi thao tác. Điều đó không đúng vì một thao tác có thể sửa đồng thời nhiều phần tử bị đặt sai vị trí nếu chúng căn chỉnh với sự phân chia ngưỡng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng trực tiếp quá trình này. Từ hoán vị hiện tại, hãy thử mọi x có thể từ 2 đến n, áp dụng thao tác phân vùng ổn định và tìm kiếm đệ quy các bước tối thiểu cho đến khi đạt được mảng đã sắp xếp. Mỗi thao tác có chi phí O(n) và không gian trạng thái là tất cả các hoán vị, làm cho trường hợp xấu nhất có kích thước giai thừa. Ngay cả việc hạn chế BFS trên các trạng thái cũng mang lại n! về nguyên tắc là không thể thực hiện được. 

Quan sát chính là hoạt động này về cơ bản là xây dựng một hệ thống phân cấp các ngưỡng giá trị. Mỗi thao tác chọn một điểm cắt trong không gian giá trị, không phải không gian vị trí. Nếu chúng ta nghĩ về cách mảng được sắp xếp cuối cùng phải xuất hiện, thì mỗi giá trị cuối cùng phải được tách biệt khỏi các giá trị lớn hơn xuất hiện trước nó. 

Cái nhìn sâu sắc về cấu trúc quan trọng là quét các giá trị từ n xuống 1 và đếm số lần “thứ tự vị trí” của các giá trị liên tiếp bị phá vỡ theo một cách cụ thể. Ở trạng thái cuối cùng chính xác, các giá trị từ 1 đến n xuất hiện theo thứ tự tăng dần về vị trí. Bất kỳ sự đảo ngược nào giữa các giá trị liên tiếp i và i+1 đều ngụ ý rằng chúng hiện ở trong các khối tương đối khác nhau và không thể sửa được nếu không có thao tác tách chúng ra. 

Mỗi thao tác có thể hợp nhất nhiều phân đoạn chính xác liền kề theo thứ tự giá trị thành một cấu trúc được sắp xếp, nhưng nó không thể sửa tất cả các dấu ngắt cùng một lúc. Điều này dẫn đến ý tưởng rằng câu trả lời được quyết định bởi số lần vị trí của i+1 xuất hiện trước i, điều này buộc phải có ít nhất một ranh giới hoạt động mới. 

Vì vậy, thay vì mô phỏng các hoạt động, chúng tôi đo lường mức độ phân mảnh của hoán vị đối với thứ tự giá trị tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Quét đơn hàng giá trị tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng pos trong đó pos[v] cho biết chỉ số của giá trị v trong hoán vị. 

Điều này chuyển đổi vấn đề từ làm việc trên các vị trí sang làm việc hoàn toàn trên các ràng buộc thứ tự giá trị. 
2. Khởi tạo một phân đoạn bộ đếm thành 1, biểu thị ít nhất một cấu trúc liền kề mà chúng ta phải hình thành. 
3. Lặp lại v từ 1 đến n-1, so sánh pos[v] và pos[v+1]. 

Mỗi lần pos[v] lớn hơn pos[v+1] nghĩa là v xuất hiện sau v+1 trong hoán vị, phá vỡ cấu trúc tăng dần. 
4. Bất cứ khi nào có sự ngắt quãng như vậy, hãy tăng các phân đoạn lên 1. 

Điều này phản ánh rằng v và v+1 không thể thuộc cùng một “khối đã được sắp xếp chính xác”, do đó cần có ít nhất một thao tác bổ sung để phân tách và sửa thứ tự. 
5. Các đoạn đầu ra trừ 1 là câu trả lời. 

Phép trừ một xuất phát từ thực tế là mảng ban đầu đã được coi là một phân đoạn trước khi áp dụng bất kỳ thao tác nào và mỗi lần ngắt được phát hiện sẽ tăng số lần hợp nhất cần thiết giữa các phân đoạn. 

### Tại sao nó hoạt động 

Mỗi thao tác với ngưỡng x sẽ chia các giá trị thành hai nhóm đơn điệu: các giá trị nhỏ hơn x và các giá trị ít nhất là x, duy trì trật tự bên trong. Điều này có nghĩa là cách duy nhất để khắc phục sự rối loạn giữa các giá trị liên tiếp theo thứ tự được sắp xếp là đảm bảo rằng cuối cùng tất cả các giá trị trước đó xuất hiện trước các giá trị sau theo thứ tự vị trí. 

Mỗi lần pos[v] > pos[v+1], cặp (v, v+1) bị đảo ngược theo thứ tự vị trí, buộc chúng thành các lớp cấu trúc khác nhau và không thể thống nhất nếu không có ít nhất một ranh giới hoạt động. Số lần phá vỡ cấu trúc như vậy xác định cần có bao nhiêu sự hợp nhất độc lập và mỗi thao tác có thể giảm chính xác một lớp phân mảnh trong trường hợp tốt nhất. 

Do đó, số lần nghỉ này tương ứng trực tiếp với số lượng thao tác tối thiểu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        
        pos = [0] * (n + 1)
        for i, v in enumerate(p):
            pos[v] = i
        
        segments = 1
        for v in range(1, n):
            if pos[v] > pos[v + 1]:
                segments += 1
        
        print(segments - 1)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là mảng đảo ngược vị trí. Bằng cách chuyển đổi giá trị thành vị trí, chúng tôi tránh mô phỏng hoàn toàn bất kỳ hoạt động nào. 

Vòng lặp v kiểm tra xem các giá trị liên tiếp có xuất hiện theo đúng thứ tự từ trái sang phải hay không. Mỗi vi phạm sẽ tăng số lượng phân đoạn cấu trúc được yêu cầu. Phép trừ cuối cùng chiếm đường cơ sở của đoạn ban đầu. 

Phải cẩn thận để xây dựng vị trí một cách chính xác vì lỗi lập chỉ mục ở đây sẽ đảo ngược hoàn toàn logic. Ngoài ra, vì có nhiều trường hợp thử nghiệm nên tất cả các mảng phải được xây dựng lại cho mỗi trường hợp để tránh rò rỉ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hoán vị đầu vào:`[2, 1, 3, 4]`Chúng tôi tính toán các vị trí: 

pos[1] = 1, pos[2] = 0, pos[3] = 2, pos[4] = 3 

| v | vị trí[v] | vị trí[v+1] | phá vỡ? | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | vâng | 2 | 
| 2 | 0 | 2 | không | 2 | 
| 3 | 2 | 3 | không | 2 | 

Đáp án là 2 - 1 = 1. 

Điều này cho thấy một sự đảo ngược duy nhất giữa 1 và 2, yêu cầu một thao tác. 

### Ví dụ 2 

Hoán vị đầu vào:`[3, 2, 1]`Vị trí: 

pos[1] = 2, pos[2] = 1, pos[3] = 0 

| v | vị trí[v] | vị trí[v+1] | phá vỡ? | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | vâng | 2 | 
| 2 | 1 | 0 | vâng | 3 | 

Đáp án là 3 - 1 = 2. 

Mỗi phép đảo ngược liền kề đóng góp một yêu cầu phân chia cấu trúc riêng biệt, phản ánh rằng hoán vị được đảo ngược hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Xây dựng mảng vị trí và quét từng giá trị | 
| Không gian | O(n) | Lưu trữ bản đồ vị trí | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng n trên tất cả các trường hợp thử nghiệm tối đa là 100.000, giúp việc quét tuyến tính cho mỗi thử nghiệm trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        pos = [0] * (n + 1)
        for i, v in enumerate(p):
            pos[v] = i
        segments = 1
        for v in range(1, n):
            if pos[v] > pos[v + 1]:
                segments += 1
        out.append(str(segments - 1))
    return "\n".join(out)

# provided samples
assert run("5\n1\n1\n2\n2 1\n6\n6 4 3 5 2 1\n3\n3 1 2\n19\n10 19 7 1 17 11 8 5 12 9 4 18 14 2 6 15 3 16 13") == "0\n1\n4\n1\n7"

# custom cases
assert run("1\n1\n1\n") == "0"
assert run("1\n4\n1 2 3 4\n") == "0"
assert run("1\n3\n2 1 3\n") == "1"
assert run("1\n5\n5 4 3 2 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`| 0 | Vỏ cạnh tối thiểu | 
|`[1 2 3 4]`| 0 | Trường hợp đã được sắp xếp | 
|`[2 1 3]`| 1 | Đảo ngược cục bộ đơn | 
|`[5 4 3 2 1]`| 4 | Hành vi phân mảnh tối đa | 

## Vỏ cạnh 

Đối với một hoán vị đã được sắp xếp như`[1, 2, 3, 4]`, mọi cặp liên tiếp đều thỏa mãn pos[v] < pos[v+1], do đó không xảy ra ngắt đoạn và thuật toán cho kết quả bằng 0. Điều này phù hợp với thực tế là không cần thực hiện thao tác nào. 

Đối với một hoán vị hoàn toàn đảo ngược như`[n, n-1, ..., 1]`, mọi cặp liền kề đều bị đảo ngược theo thứ tự vị trí, do đó số đoạn trở thành n, tạo ra câu trả lời n-1. Mỗi phép đảo ngược tạo ra sự phân tách cấu trúc riêng biệt và thuật toán sẽ tích lũy chính xác tất cả các hoạt động cần thiết. 

Đối với một hoán vị với một trao đổi cục bộ duy nhất, chẳng hạn như`[1, 3, 2, 4]`, chỉ có cặp (2, 3) tạo ra điểm ngắt, tạo ra một thao tác bắt buộc duy nhất. Quá trình quét phát hiện chính xác một đảo ngược vị trí và thuật toán chuyển trực tiếp điều đó thành một bước phân vùng.
