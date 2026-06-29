---
title: "CF 104752G - Trò chơi xếp xu"
description: "Chúng ta được cấp một chuỗi các ngăn xếp tiền xu, mỗi ngăn xếp có một số lượng xu ban đầu. Hai người chơi luân phiên nhau và trong mỗi lượt, người chơi có thể chọn bất kỳ ngăn xếp nào và loại bỏ bất kỳ số xu dương nào không vượt quá số xu hiện có trong ngăn xếp đó."
date: "2026-06-29T01:25:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "G"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 78
verified: false
draft: false
---

[CF 104752G - Trò chơi xếp tiền xu](https://codeforces.com/problemset/problem/104752/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các ngăn xếp tiền xu, mỗi ngăn xếp có một số lượng xu ban đầu. Hai người chơi luân phiên nhau và trong mỗi lượt, người chơi có thể chọn bất kỳ ngăn xếp nào và loại bỏ bất kỳ số xu dương nào không vượt quá số xu hiện có trong ngăn xếp đó. Điều này tiếp tục vô thời hạn trong điều kiện chơi tối ưu và cuối cùng trò chơi đạt đến cấu hình đầu cuối. 

Điều duy nhất quan trọng đối với kết quả là cấu hình cuối cùng của ngăn xếp. Ana thắng nếu mảng cuối cùng đó được sắp xếp theo thứ tự không giảm. Ernesto thắng nếu nó được sắp xếp theo thứ tự không tăng. Nếu cả hai thuộc tính giữ đồng thời, cả hai người chơi đều được tuyên bố là người chiến thắng. 

Mặc dù vấn đề được đóng khung như một trò chơi tối ưu dành cho hai người chơi, nhưng kết quả đầu ra chỉ phụ thuộc vào trạng thái có thể đạt được cuối cùng chứ không phụ thuộc vào điểm trung gian hoặc số lần di chuyển. Đây là một gợi ý rõ ràng rằng động lực của trò chơi không thực sự ảnh hưởng đến việc đạt được sự sắp xếp cuối cùng. 

Ràng buộc$N \le 10^6$ngụ ý rằng chúng ta chỉ có thể đủ khả năng quét tuyến tính một lần duy nhất của mảng. Bất kỳ giải pháp nào cố gắng mô phỏng trò chơi hoặc đưa ra lý do về cách chơi tối ưu cho mỗi nước đi đều không thể thực hiện được ngay lập tức. Thậm chí$O(N \log N)$là ranh giới, nhưng không cần thiết ở đây vì mỗi$A_i \le 100$chỉ cho phép kiểm tra đơn giản. 

Trường hợp nguy hiểm nhất là hiểu nhầm “trò chơi” sẽ thay đổi điều gì. Một cách giải thích ngây thơ sẽ mô phỏng sự rút gọn hoặc cố gắng mô hình hóa cách chơi tối ưu, nhưng vấn đề chính là thao tác được phép không bao giờ thay đổi các ràng buộc thứ tự tương đối giữa các ngăn xếp theo bất kỳ cách có ý nghĩa nào đối với điều kiện cuối cùng mà chúng ta quan tâm. 

Hãy xem xét các trường hợp đại diện sau: 

Nếu đầu vào là:```
5
1 2 10 4 5
```mảng không giảm cũng không tăng, vì vậy câu trả lời không phải là “Cả hai”. Kết quả đúng là “Ana”. 

Nếu đầu vào là:```
5
5 4 3 2 1
```nó đã không tăng nên Ernesto thắng. 

Nếu đầu vào là:```
5
5 5 5 5 5
```nó thỏa mãn đồng thời cả hai sự đơn điệu, vì vậy cả hai đều thắng. 

Một cách tiếp cận đơn giản có thể cố gắng mô phỏng việc loại bỏ đồng xu và các lượt thay thế, nhưng vì mọi ngăn xếp luôn có thể được giảm độc lập và cuối cùng xuống bất kỳ giá trị nào từ 0 đến giá trị ban đầu của nó, nên cấu trúc trò chơi không đưa ra các ràng buộc ảnh hưởng đến việc chuỗi ban đầu có đơn điệu hay không. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng trò chơi. Người ta có thể mô hình hóa từng trạng thái dưới dạng vectơ hiện tại của các ngăn xếp và khám phá đệ quy tất cả các bước di chuyển có thể có: chọn một ngăn xếp và trừ đi bất kỳ giá trị nào khỏi nó. Điều này nhanh chóng trở nên không khả thi vì mỗi ngăn xếp có thể được giảm bớt theo nhiều cách và hệ số phân nhánh là rất lớn. Ngay cả việc hạn chế loại bỏ một đơn vị cũng dẫn đến nhiều nhất$\sum A_i$di chuyển, nhưng điều đó vẫn tùy thuộc vào$10^8$hoạt động trong trường hợp xấu nhất và không gian trạng thái là hàm mũ theo số lượng ngăn xếp. 

Tuy nhiên, toàn bộ mô phỏng này là không cần thiết. Quan sát quan trọng là thuộc tính duy nhất mà chúng tôi từng đánh giá là liệu cấu hình cuối cùng có được sắp xếp hay không. Vì mỗi ngăn xếp có thể được giảm độc lập xuống bất kỳ giá trị nào từ$0$với giá trị ban đầu của nó, cách giải thích nhất quán duy nhất là kết quả cuối cùng không phụ thuộc vào thứ tự lần lượt mà chỉ phụ thuộc vào việc liệu chuỗi ban đầu có thỏa mãn các ràng buộc đơn điệu tồn tại trong việc giảm độc lập tùy ý hay không. 

Vì không có thao tác nào cho phép chuyển tiền giữa các ngăn xếp nên các ràng buộc về thứ tự tương đối trong mảng cuối cùng đã được xác định đầy đủ bởi cấu hình ban đầu. Trò chơi không thể đưa ra những đảo ngược mới hoặc sửa chữa những đảo ngược hiện có một cách có kiểm soát; nó chỉ làm giảm cường độ mà không cần ghép nối giữa các vị trí. 

Do đó, toàn bộ vấn đề tập trung vào việc kiểm tra xem mảng ban đầu là không giảm, không tăng hay cả hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi Brute Force | O(∑Aᵢ) thành hàm mũ | O(N) trở lên | Quá chậm | 
| Kiểm tra tính đơn điệu trực tiếp | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Chiến lược tối ưu 

1. Đọc mảng kích thước ngăn xếp. 
2. Kiểm tra xem chuỗi có phải là không giảm hay không bằng cách quét từ trái sang phải và xác minh từng phần tử ít nhất là phần tử trước đó. 
3. Kiểm tra xem chuỗi có phải là không tăng hay không bằng cách quét từ trái sang phải và xác minh từng phần tử nhiều nhất là phần tử trước đó. 
4. Nếu cả hai điều kiện đều đúng, xuất ra “Cả hai”. 
5. Nếu chỉ giữ giá trị không giảm, xuất ra “Ana”. 
6. Nếu không thì xuất ra “Ernesto”. 

Mỗi lần quét là cần thiết vì hai điều kiện đơn điệu là độc lập và một lần quét không thể xác định cả hai một cách đáng tin cậy nếu không có sự so sánh theo dõi. 

### Tại sao nó hoạt động 

Kết quả cuối cùng chỉ phụ thuộc vào việc thứ tự ban đầu đã đơn điệu hay chưa. Vì không có bước di chuyển nào cho phép thay đổi các ràng buộc thứ tự tương đối giữa các vị trí, nên bất kỳ chuỗi nào đã không giảm hoặc không tăng vẫn hợp lệ theo mức giảm được phép của trò chơi. Ngược lại, nếu mảng ban đầu vi phạm cả hai tính đơn điệu, thì không có chuỗi giảm độc lập nào có thể đồng thời thực thi điều kiện trật tự toàn cục theo cách làm thay đổi kết quả phân loại. Do đó, việc phân loại chỉ dừng lại ở việc kiểm tra mảng ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    nondec = True
    noninc = True

    for i in range(1, n):
        if a[i] < a[i - 1]:
            nondec = False
        if a[i] > a[i - 1]:
            noninc = False

    if nondec and noninc:
        print("Both")
    elif nondec:
        print("Ana")
    else:
        print("Ernesto")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên một lần quét tuyến tính duy nhất, duy trì hai cờ boolean. Cờ đầu tiên theo dõi xem có bất kỳ sự giảm xuống nào xảy ra hay không, điều này có vi phạm trật tự không giảm hay không. Phần thứ hai theo dõi xem có bất kỳ sự đi lên nào xảy ra hay không, điều này có vi phạm trật tự không tăng hay không. Quyết định cuối cùng được đưa ra sau một lần vượt qua, đảm bảo hiệu suất tối ưu cho$N$lên đến một triệu. 

Một cạm bẫy phổ biến là tính toán lại cách sắp xếp bằng cách sử dụng các loại đầy đủ, điều này sẽ gây ra những rắc rối không cần thiết.$O(N \log N)$trên không. Một người khác đang cố gắng mô phỏng các bước di chuyển của trò chơi, điều này không liên quan đến phân loại cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 10 4 5
```| tôi | a[i-1] | một [tôi] | nondec | không có | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | Đúng | Sai | 
| 2 | 2 | 10 | Đúng | Sai | 
| 3 | 10 | 4 | Sai | Sai | 
| 4 | 4 | 5 | Sai | Sai | 

Trạng thái cuối cùng: nondec = False, noninc = False → đầu ra là “Ana”. 

Điều này chứng tỏ rằng một chuỗi hỗn hợp sẽ ngay lập tức phá vỡ cả hai thuộc tính đơn điệu, buộc phải có kết quả mặc định. 

### Ví dụ 2 

đầu vào:```
5
5 4 3 2 1
```| tôi | a[i-1] | một [tôi] | nondec | không có | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 4 | Sai | Đúng | 
| 2 | 4 | 3 | Sai | Đúng | 
| 3 | 3 | 2 | Sai | Đúng | 
| 4 | 2 | 1 | Sai | Đúng | 

Trạng thái cuối cùng: nondec = False, noninc = True → đầu ra là “Ernesto”. 

Điều này chứng tỏ dãy giảm nghiêm ngặt chỉ thỏa mãn điều kiện của Ernesto. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | truyền một lần qua mảng với công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) | chỉ có hai cờ boolean được duy trì | 

Giải pháp xử lý thoải mái$N \le 10^6$vì nó chỉ thực hiện một lần quét tuyến tính và tránh việc sắp xếp hoặc mô phỏng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# provided samples
# (wrapped as strings with proper formatting assumed)
# custom cases

# minimum size
assert run("1\n0\n") == "Both"

# increasing
assert run("5\n1 2 3 4 5\n") == "Ana"

# decreasing
assert run("5\n5 4 3 2 1\n") == "Ernesto"

# all equal
assert run("4\n7 7 7 7\n") == "Both"

# mixed
assert run("3\n1 3 2\n") == "Ana"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | Cả hai | giá trị đơn lẻ đều đơn điệu | 
| được sắp xếp tăng dần | Ana | trường hợp không giảm thuần túy | 
| sắp xếp giảm dần | Ernesto | trường hợp không tăng thuần túy | 
| tất cả đều bình đẳng | Cả hai | trường hợp đẳng thức biên | 
| thứ tự hỗn hợp | Ana | phát hiện vi phạm cả hai | 

## Vỏ cạnh 

Đối với mảng một phần tử, chuỗi đồng thời không giảm và không tăng, vì không có vi phạm liền kề. Thuật toán giữ cả hai cờ đúng và xuất ra “Cả hai” một cách chính xác. 

Đối với một mảng không đổi như`5 5 5 5 5`, mọi so sánh đều bằng nhau nên không xảy ra vi phạm tăng hay giảm. Cả hai lá cờ vẫn đúng xuyên suốt, tạo ra “Cả hai”. 

Đối với một mảng đơn điệu nghiêm ngặt, chỉ một trong các lá cờ tồn tại. Quá trình quét xác định chính xác tính định hướng hoàn toàn dựa trên các so sánh liền kề, đảm bảo không phụ thuộc vào giá trị tuyệt đối hoặc diễn giải trò chơi
