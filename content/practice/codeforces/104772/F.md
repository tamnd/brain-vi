---
title: "CF 104772F - Được giải quyết lần đầu, được mã hóa lần cuối"
description: "Chúng ta có hai chuỗi có độ dài n mô tả cùng một tập hợp các chủ đề bài toán. Trình tự đầu tiên mô tả thứ tự các giải pháp có sẵn, từng giải pháp một và mỗi giải pháp mới được đặt vào một ngăn xếp."
date: "2026-06-28T15:40:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104772
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104772
solve_time_s: 64
verified: true
draft: false
---

[CF 104772F - Được giải quyết lần đầu, được mã hóa lần cuối](https://codeforces.com/problemset/problem/104772/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài n mô tả cùng một tập hợp các chủ đề bài toán. Trình tự đầu tiên mô tả thứ tự các giải pháp có sẵn, từng giải pháp một và mỗi giải pháp mới được đặt vào một ngăn xếp. Trình tự thứ hai mô tả thứ tự chính xác mà các giải pháp này phải được lấy từ ngăn xếp và xử lý. 

Tại mỗi thời điểm, chúng ta đẩy phần tử có sẵn tiếp theo từ chuỗi đầu tiên vào ngăn xếp hoặc bật phần tử trên cùng của ngăn xếp và xuất nó dưới dạng phần tử bắt buộc tiếp theo của chuỗi thứ hai. Mục tiêu là để quyết định xem có thể thực hiện một chuỗi chính xác n lần đẩy và n lần bật sao cho chuỗi xuất hiện khớp chính xác với mảng thứ hai hay không và nếu vậy, hãy xây dựng một chuỗi thao tác như vậy. 

Ràng buộc n 100 có nghĩa là bất kỳ giải pháp mô phỏng bậc hai hoặc thậm chí đơn giản nào với các phép toán ngăn xếp tuyến tính đều dễ dàng đủ nhanh. Không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài mô phỏng tham lam đơn giản. Điều tinh tế duy nhất là tính chính xác đối với các bản sao, vì các giá trị không khác biệt và trực giác “hoán vị ngăn xếp từ 1 đến n” thông thường phải được điều chỉnh cẩn thận. 

Một nỗ lực ngây thơ có thể cố gắng quay lại tất cả các lần đẩy và bật có thể xen kẽ. Cách tiếp cận đó phân nhánh ở mỗi bước, dẫn đến số lượng trạng thái theo cấp số nhân. Ngay cả với n = 100 điều này vẫn hoàn toàn không khả thi. 

Một cách tiếp cận sai lầm tinh vi hơn là luôn đẩy mọi thứ lên trước rồi cố gắng bật theo thứ tự. Điều đó rõ ràng không thành công khi đầu ra mong muốn yêu cầu bật sớm trước khi các phần tử sau được đẩy. 

Mẫu trường hợp cạnh khóa là khi đầu ra được yêu cầu tiếp theo nằm sâu hơn trong chuỗi đầu vào trong tương lai, nhưng một phần tử khác hiện đang chặn nó trên ngăn xếp. Nếu chúng tôi bật sớm phần tử sai, chúng tôi có thể chặn vĩnh viễn đơn hàng được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét mọi chuỗi hoạt động S và C, duy trì ngăn xếp hiện tại và kiểm tra xem đầu ra được tạo ra có phù hợp với mục tiêu hay không. Vì mỗi vị trí trong số 2n vị trí có thể là S hoặc C với các ràng buộc, nên số lượng chuỗi hợp lệ tăng lên theo kiểu tổ hợp. Trong trường hợp xấu nhất, điều này khám phá theo thứ tự của các cấu trúc giống số Catalan, tăng theo cấp số nhân với n. Điều này nhanh chóng trở nên không thể ngay cả khi n = 30. 

Quan sát quan trọng là quá trình này có cấu trúc tham lam mạnh mẽ. Khi một phần tử ở trên cùng của ngăn xếp và nó khớp với đầu ra được yêu cầu tiếp theo, việc trì hoãn việc loại bỏ nó không bao giờ có ích. Bất kỳ lựa chọn nào khác sẽ chỉ chôn vùi nó sâu hơn và có nguy cơ chặn các yếu tố cần thiết trong tương lai. Điều này ngụ ý rằng bất cứ khi nào đỉnh ngăn xếp khớp với phần tử cần thiết tiếp theo, chúng ta phải bật ngay lập tức. 

Với ý tưởng đó, toàn bộ quá trình sẽ trở thành một lần quét từ trái sang phải của chuỗi đầu vào, mô phỏng các lần đẩy, trong khi liên tục bật lên bất cứ khi nào có thể. Điều này làm giảm vấn đề duy trì ngăn xếp và con trỏ vào chuỗi mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{2n}) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quá trình bằng cách sử dụng một ngăn xếp, hai con trỏ và một chuỗi kết quả.

1. Khởi tạo một ngăn xếp trống và đặt con trỏ j = 0 cho chuỗi mục tiêu. 
2. Lặp lại i từ 0 đến n − 1 trên chuỗi nguồn. Với mỗi phần tử, đẩy nó vào ngăn xếp và ghi lại thao tác 'S'. Việc đẩy là cần thiết vì chúng tôi chỉ có thể truy cập các phần tử thông qua cấu trúc ngăn xếp và chúng tôi phải cung cấp phần tử nguồn hiện tại cho các kết quả phù hợp tiềm năng trong tương lai. 
3. Sau mỗi lần đẩy, hãy kiểm tra lại phần trên cùng của ngăn xếp. Trong khi ngăn xếp không trống và phần trên cùng bằng phần tử đích hiện tại b[j], hãy lấy nó ra khỏi ngăn xếp, ghi 'C' và tăng j. Việc loại bỏ tham lam này đảm bảo chúng tôi không bao giờ trì hoãn kết quả khớp chính xác có sẵn vì việc trì hoãn chỉ thêm các phần tử chặn phía trên kết quả đó. 
4. Tiếp tục quá trình này cho đến khi tất cả các phần tử nguồn đã được đẩy. 
5. Sau khi hoàn thành tất cả các lần đẩy, nếu chúng ta đã khớp thành công tất cả các phần tử trong chuỗi mục tiêu (j == n), thì các thao tác được ghi lại sẽ tạo thành một giải pháp hợp lệ. Nếu không thì không thể được. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ điểm nào trong mô phỏng, ngăn xếp biểu thị chính xác tập hợp các phần tử đã được đẩy nhưng chưa xuất ra và thứ tự tương đối của chúng được cố định theo thời gian chèn. Bất cứ khi nào phần trên cùng của ngăn xếp khớp với đầu ra được yêu cầu tiếp theo, việc loại bỏ nó ngay lập tức luôn là an toàn và tối ưu vì việc rời khỏi nó sẽ chỉ làm trì hoãn một kết quả khớp hợp lệ trong khi có khả năng đưa ra các phần tử chặn mới phía trên nó. Nếu quá trình kết thúc với các phần tử chưa khớp trong chuỗi mục tiêu, điều đó có nghĩa là một số phần tử bắt buộc không bao giờ được hiển thị ở đầu ngăn xếp vào đúng thời điểm, điều này không thể khắc phục được bằng các thao tác sắp xếp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    stack = []
    res = []
    j = 0
    
    for x in a:
        stack.append(x)
        res.append('S')
        
        while stack and j < n and stack[-1] == b[j]:
            stack.pop()
            res.append('C')
            j += 1
    
    if j == n:
        print("YES")
        print("".join(res))
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Ngăn xếp lưu trữ tất cả các giải pháp hiện có. Sau mỗi lần đẩy, vòng lặp bên trong sẽ loại bỏ bất kỳ phần tử nào đáp ứng ngay lập tức đầu ra được yêu cầu tiếp theo. Con trỏ j đảm bảo chúng ta không bao giờ bỏ qua hoặc sắp xếp lại thứ tự đích. Lần kiểm tra cuối cùng đảm bảo tất cả các đầu ra cần thiết đều được tạo ra; mặt khác, một số phần tử đã bị chặn vĩnh viễn theo thứ tự ngăn xếp. 

Một điểm tinh tế là các bản sao được xử lý một cách tự nhiên. Vì việc so khớp được thực hiện theo giá trị chứ không phải theo danh tính nên nhiều giá trị giống hệt nhau được xử lý thay thế cho nhau và quy tắc tham lam vẫn được giữ vì tính chính xác chỉ phụ thuộc vào thứ tự ngăn xếp chứ không phụ thuộc vào tính duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 4 

a = [4, 1, 2, 2] 

b = [1, 2, 4, 2] 

| Bước | Hành động | Ngăn xếp | j | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | Đẩy 4 | [4] | 0 | S | 
| 2 | Đẩy 1 | [4,1] | 0 | SS | 
| 3 | Pop 1 | [4] | 1 | SSC | 
| 4 | Đẩy 2 | [4,2] | 1 | SSCS | 
| 5 | Pop 2 | [4] | 2 | SSCSC | 
| 6 | Đẩy 2 | [4,2] | 2 | SSCSCS | 
| 7 | Pop 2 | [4] | 3 | SSCSCSC | 
| 8 | Pop 4 | [] | 4 | SSCSCCSC | 

Dấu vết này cho thấy cách bật ngay lập tức bất cứ khi nào phần trên khớp với mục tiêu sẽ đảm bảo không xảy ra tình trạng chặn không cần thiết. Mỗi khi một trận đấu xuất hiện, nó sẽ được giải quyết ngay lập tức. 

### Ví dụ 2 

đầu vào: 

n = 3 

a = [2, 3, 1] 

b = [1, 2, 3] 

Chúng ta đẩy 2 (ngăn xếp [2]), đẩy 3 (ngăn xếp [2,3]) và cuối cùng phải xuất ra 1. Tuy nhiên, 1 không bao giờ xuất hiện trên đầu ngăn xếp vào đúng thời điểm; nó đến quá muộn sau khi 2 và 3 đã chặn nó. Quá trình kết thúc với ngăn xếp [2,3,1], nhưng chúng ta không thể bật 1 trước 3 và 2, khiến cho thứ tự được yêu cầu là không thể. 

Điều này chứng tỏ rằng mặc dù cả hai chuỗi đều chứa cùng nhiều tập hợp, nhưng các ràng buộc ngăn xếp có thể khiến việc sắp xếp không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy một lần và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | Ngăn xếp chứa tối đa n phần tử | 

Độ phức tạp tuyến tính dễ dàng đủ với n ≤ 100. Thuật toán thực hiện một lượng công việc không đổi trên mỗi phần tử và không liên quan đến tìm kiếm lồng nhau hoặc quay lui. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input().strip())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    stack = []
    res = []
    j = 0
    
    for x in a:
        stack.append(x)
        res.append('S')
        while stack and j < n and stack[-1] == b[j]:
            stack.pop()
            res.append('C')
            j += 1
    
    if j == n:
        return "YES\n" + "".join(res)
    return "NO"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("""4
4 1 2 2
1 2 4 2
""") == "YES\nSSCSCCSC"

assert run("""3
2 3 1
1 2 3
""") == "NO"

# custom cases
assert run("""1
1
1
""") == "YES\nSC"

assert run("""2
1 2
2 1
""") == "YES\nSSCC"

assert run("""3
1 1 1
1 1 1
""") == "YES\nSCSC SC".replace(" ", "")

assert run("""4
1 2 3 4
4 3 2 1
""") == "YES\nSSSSCCCC"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử giống nhau | SC | Độ chính xác kích thước tối thiểu | 
| 1 2 → 2 1 | SSCC | Hành vi ngăn xếp đảo ngược cơ bản | 
| tất cả những cái | SCSC… | Xử lý trùng lặp | 
| trình tự đảo ngược | SSSSCCCC | Độ sâu ngăn xếp cực cao | 

## Vỏ cạnh 

Trường hợp khó phát hiện khi có nhiều giá trị giống nhau xuất hiện. Thuật toán vẫn hoạt động chính xác vì so sánh đẳng thức không phụ thuộc vào vị trí và mọi lần xuất hiện đều không thể phân biệt được về mặt khả thi. Cấu trúc ngăn xếp chỉ xác định tính chính xác. 

Một trường hợp khác là khi chuỗi mục tiêu yêu cầu một phần tử trước khi nó được đẩy. Trong tình huống đó, mô phỏng tham lam sẽ đẩy mọi thứ cho đến khi nó có thể truy cập được ở trên cùng hoặc vẫn bị chặn. Nếu nó không bao giờ tới đúng vị trí trên cùng, con trỏ cuối cùng j sẽ không tới được n, báo hiệu chính xác sự không thể xảy ra.
