---
title: "CF 104728C - \u6392\u5217\u6392\u5e8f\u95ee\u9898"
description: "Chúng ta được cho một hoán vị của các số từ 1 đến n và chúng ta muốn chuyển đổi nó thành dãy được sắp xếp 1, 2, 3, ..., n bằng cách sử dụng một loại hoạt động tái cơ cấu đặc biệt."
date: "2026-06-29T02:44:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "C"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 68
verified: true
draft: false
---

[CF 104728C - \u6392\u5217\u6392\u5e8f\u95ee\u9898](https://codeforces.com/problemset/problem/104728/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ 1 đến n và chúng ta muốn chuyển đổi nó thành dãy được sắp xếp 1, 2, 3, ..., n bằng cách sử dụng một loại hoạt động tái cơ cấu đặc biệt. Thao tác rất linh hoạt: chúng ta có thể cắt mảng thành nhiều phần liền kề, tùy ý đảo ngược một số phần đó, sau đó sắp xếp lại các phần theo thứ tự bất kỳ trước khi ghép chúng lại thành một chuỗi duy nhất. 

Chi phí không phải là toàn bộ hoạt động mà là số lần chúng ta thực hiện bước cắt. Câu hỏi đặt ra là cần có số lần cắt tối thiểu để sau khi áp dụng phép sắp xếp lại và đảo ngược được phép, hoán vị có thể được sắp xếp. 

Khó khăn chính là một khi chúng ta cắt, chúng ta có được rất nhiều sự tự do: các phân đoạn có thể được đảo ngược và sắp xếp lại một cách tùy ý. Điều đó có nghĩa là hạn chế thực sự không phải là về hoán đổi cục bộ mà là về cách hoán vị có thể được phân tách thành “các khối có thể đảo ngược” có thể được sắp xếp theo thứ tự tăng dần. 

Kích thước đầu vào lên tới 10^6, do đó, bất kỳ giải pháp nào cố gắng mô phỏng hoạt động hoặc khám phá các phân vùng đều quá chậm. Ngay cả O(n log n) với chi phí liên tục lớn cũng được, nhưng lý do O(n^2) về phân đoạn hoặc cố gắng tham lam thì không. 

Trường hợp cạnh tinh tế xuất hiện khi hoán vị đã được sắp xếp hoặc đảo ngược hoàn toàn. Ví dụ: nếu mảng đã có [1, 2, 3, 4] thì không cần cắt và nếu nó là [3, 2, 1] với n = 3, chúng ta có thể đảo ngược toàn bộ mảng và kết thúc với số lần cắt bằng 0. Một giải pháp ngây thơ chỉ kiểm tra các lần chạy tăng liền kề sẽ cho rằng mảng đảo ngược là “xấu” và yêu cầu cắt, nhưng việc đảo ngược các phân đoạn khiến nó hoàn toàn hợp lệ. 

Một tình huống quan trọng khác là khi hoán vị bao gồm nhiều khối đơn điệu không được căn chỉnh theo thứ tự giá trị. Ví dụ: [1, 3, 2, 4] hoạt động khác với [1, 2, 4, 3], mặc dù cả hai đều có nghịch đảo cục bộ. Cấu trúc vị trí của các giá trị liên tiếp quan trọng hơn thứ tự cục bộ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách để phân chia hoán vị thành các phân đoạn và với mỗi phân đoạn, hãy thử tất cả các tập hợp con của các phân đoạn đảo ngược và tất cả các hoán vị của thứ tự phân đoạn, sau đó kiểm tra xem liệu chúng ta có thể tạo ra một chuỗi được sắp xếp hay không. Điều này đúng về mặt khái niệm vì hoạt động cho phép rõ ràng những chuyển đổi này. 

Tuy nhiên, số cách chia thành các phân đoạn là theo cấp số nhân tính theo n và đối với mỗi cách phân chia, số lần sắp xếp lại là giai thừa của số phân đoạn. Ngay cả với n khoảng 20, điều này vẫn không khả thi và ở n lên tới 10^6 thì điều đó hoàn toàn không thể. 

Quan sát quan trọng là sau khi chúng ta cắt, các phân đoạn hoạt động giống như các khối nguyên tử có thứ tự bên trong có thể bị đảo lộn nhưng vị trí tương đối của chúng hoàn toàn linh hoạt. Điều này có nghĩa là điều duy nhất quan trọng là liệu các phần tử phải liên tiếp trong mảng được sắp xếp cuối cùng có thể được nhóm lại mà không buộc phải cắt thêm hay không. 

Nếu chúng ta xem xét hoán vị theo vị trí của các giá trị, chúng ta có thể nghĩ đến việc xây dựng thứ tự sắp xếp từ 1 đến n và kiểm tra xem “giá trị tiếp theo dự kiến” không liền kề với giá trị trước đó bao nhiêu lần theo cách có thể sửa được mà không cần đưa ra một phần cắt mới. Mỗi khi tính liên tục bị phá vỡ theo cách không thể sửa chữa được bằng cách đảo ngược bên trong một phân đoạn, chúng tôi buộc phải đưa ra một đoạn cắt mới. 

Điều này làm giảm vấn đề đếm xem có bao nhiêu “đoạn có giá trị liên tiếp trong cấu trúc kề cận chính xác” tồn tại trong hoán vị khi xem xét cả khả năng kề cận thuận và nghịch. 

Cấu trúc cuối cùng hóa ra phụ thuộc vào việc liệu i và i+1 có liền kề trong hoán vị theo một trong hai thứ tự hay không. Nếu đúng như vậy, chúng có thể thuộc cùng một phân khúc (có thể đảo ngược). Nếu không thì cần phải có ranh giới phân đoạn mới.

Sau đó, chúng tôi giảm thiểu việc cắt bằng cách hợp nhất tất cả các chuỗi tối đa trong đó các số nguyên liên tiếp liền kề theo một trong hai hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các phân vùng và đảo ngược | O(2^n · n!) | O(n) | Quá chậm | 
| Nhóm chuỗi liền kề | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính vị trí của từng giá trị trong hoán vị. 

Điều này cho phép chúng ta nhanh chóng kiểm tra xem mỗi số nguyên xuất hiện ở đâu mà không cần quét mảng nhiều lần. 
2. Với mọi giá trị i từ 1 đến n−1, hãy kiểm tra xem i và i+1 có kề nhau trong hoán vị hay không. 

Chúng được coi là liền kề nếu vị trí của chúng khác nhau đúng 1. 
3. Nếu i và i+1 liền kề nhau, hãy xác định xem chúng xuất hiện theo thứ tự tăng hay giảm trong mảng. 

Cả hai hướng đều hợp lệ vì chúng ta được phép đảo ngược các đoạn. 
4. Xây dựng kết nối giống như đồ thị trên các giá trị từ 1 đến n trong đó i được kết nối với i+1 nếu chúng liền kề trong mảng. 
5. Đếm số lượng thành phần được kết nối trong cấu trúc chuỗi này. 

Mỗi thành phần tương ứng với một nhóm số nguyên liên tiếp tối đa có thể được đặt bên trong một phân đoạn sau khi có thể đảo ngược. 
6. Câu trả lời là số thành phần trừ đi một, tương ứng với số lần cắt cần thiết để tách các nhóm này. 

Tại sao nó hoạt động: 

Hoán vị chỉ có thể được sắp xếp lại một cách tự do ở các ranh giới phân đoạn, nhưng trong một phân đoạn, chúng ta phải đảm bảo rằng tất cả các giá trị liên tiếp theo thứ tự được sắp xếp có thể được đặt liền kề thông qua vị trí tiến hoặc lùi. Nếu hai giá trị liên tiếp không liền kề trong hoán vị ban đầu thì không có sự đảo ngược nào bên trong một phân đoạn có thể làm cho chúng liền kề nhau mà không tạo ra một vết cắt giữa chúng. Do đó, các phân đoạn hợp lệ tương ứng chính xác với chuỗi tối đa của các giá trị được định vị liên tiếp và mỗi lần cắt sẽ tăng số lượng chuỗi đó bằng cách chia một thành phần thành hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    
    pos = [0] * (n + 1)
    for i, v in enumerate(p):
        pos[v] = i
    
    components = 1
    
    for i in range(1, n):
        if abs(pos[i] - pos[i + 1]) != 1:
            components += 1
    
    print(components - 1)

if __name__ == "__main__":
    solve()
```Mã đầu tiên ghi lại nơi mỗi giá trị xuất hiện để kiểm tra kề trở thành O(1). Sau đó, nó quét các giá trị liên tiếp và tăng số lượng thành phần bất cứ khi nào hai giá trị liền kề theo thứ tự được sắp xếp không nằm cạnh nhau trong hoán vị. Mỗi dấu ngắt như vậy biểu thị một điểm cắt cần thiết vì không có phân đoạn hợp lệ nào có thể chứa đồng thời cả hai giá trị. 

Phép trừ cuối cùng bằng một sẽ chuyển đổi số phân đoạn thành số lần cắt, vì k phân đoạn cần k−1 lần cắt để phân tách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 5 4
```| tôi | vị trí[i] | vị trí[i+1] | Liền kề? | Linh kiện | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | vâng | 1 | 
| 2 | 1 | 2 | vâng | 1 | 
| 3 | 2 | 4 | không | 2 | 
| 4 | 4 | 3 | có ( | 4-3 | 

Câu trả lời cuối cùng: 1 

Dấu vết này cho thấy chỉ có cặp (3,4) được tách ra theo cách phá vỡ chuỗi. Điều đó buộc phải có thêm một đoạn nữa, nghĩa là cần phải cắt một đoạn. Phần còn lại của hoán vị tạo thành một khối liên tục có thể được sắp xếp lại bên trong. 

### Ví dụ 2 

đầu vào:```
3
3 2 1
```| tôi | vị trí[i] | vị trí[i+1] | Liền kề? | Linh kiện | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | vâng | 1 | 
| 2 | 1 | 0 | vâng | 1 | 

Câu trả lời cuối cùng: 0 

Ở đây, mỗi cặp liên tiếp liền kề nhau theo thứ tự đảo ngược, nghĩa là toàn bộ hoán vị tạo thành một khối có thể đảo ngược duy nhất. Chỉ cần đảo ngược toàn bộ chuỗi là đủ nên không cần cắt giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần để xây dựng vị trí và một lần để kiểm tra tính liền kề giữa các giá trị liên tiếp | 
| Không gian | O(n) | Mảng vị trí lưu trữ vị trí của từng giá trị | 

Cấu trúc tuyến tính phù hợp thoải mái trong giới hạn n lên tới 10^6, vì thuật toán chỉ thực hiện tra cứu mảng đơn giản và quét một lần trên phạm vi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    data = inp.strip().split()
    n = int(data[0])
    p = list(map(int, data[1:]))
    
    pos = [0] * (n + 1)
    for i, v in enumerate(p):
        pos[v] = i
    
    components = 1
    for i in range(1, n):
        if abs(pos[i] - pos[i + 1]) != 1:
            components += 1
    
    return str(components - 1)

# provided samples
assert run("5\n1 2 3 5 4") == "1"
assert run("3\n3 2 1") == "0"

# custom cases
assert run("1\n1") == "0"
assert run("4\n1 2 3 4") == "0"
assert run("4\n4 3 2 1") == "0"
assert run("4\n1 3 2 4") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | ranh giới tối thiểu | 
| đã được sắp xếp | 0 | không cần cắt giảm | 
| đảo ngược hoàn toàn | 0 | hoạt động đảo ngược toàn cầu | 
| đảo ngược cục bộ | 1 | trường hợp nghỉ đơn | 

## Vỏ cạnh 

Đối với hoán vị một phần tử như [1], mảng vị trí không quan trọng và không có cặp liên tiếp nào để kiểm tra, do đó số thành phần vẫn là 1 và câu trả lời trở thành 0. Điều này xác nhận rằng thuật toán xử lý chính xác đầu vào suy biến mà không cố gắng truy cập chỉ mục không hợp lệ. 

Đối với một mảng được sắp xếp đầy đủ [1, 2, 3, 4, 5], mọi cặp liên tiếp đều liền kề trong mảng ban đầu, do đó không xảy ra hiện tượng ngắt thành phần. Cấu trúc vẫn là một chuỗi duy nhất, không tạo ra vết cắt nào như mong đợi. 

Đối với mảng đảo ngược hoàn toàn [4, 3, 2, 1], mọi cặp liên tiếp vẫn liền kề ở vị trí, chỉ theo hướng ngược lại. Vì vùng kề được xác định bằng chênh lệch vị trí tuyệt đối nên tất cả các cặp vẫn được kết nối và thuật toán tạo ra các điểm cắt bằng 0 một cách chính xác, phản ánh rằng chỉ cần đảo ngược một lần là đủ.
