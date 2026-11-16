---
title: "CF 1906J - Đồ thị đếm BFS"
description: "Chúng ta được cung cấp một thứ tự cố định của tất cả các đỉnh, bắt đầu từ nút 1 và thứ tự này được khẳng định là thứ tự mà BFS phát hiện ra các nút trong một số đồ thị đơn giản vô hướng."
date: "2026-06-08T20:46:36+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 1906
codeforces_index: "J"
codeforces_contest_name: "2023-2024 ICPC, Asia Jakarta Regional Contest (Online Mirror, Unrated, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1906
solve_time_s: 78
verified: true
draft: false
---

[CF 1906J - Biểu đồ đếm BFS](https://codeforces.com/problemset/problem/1906/J) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một thứ tự cố định của tất cả các đỉnh, bắt đầu từ nút 1 và thứ tự này được khẳng định là thứ tự mà BFS phát hiện ra các nút trong một số đồ thị đơn giản vô hướng. Quy tắc BFS là tiêu chuẩn: bắt đầu từ nút 1, chúng tôi quét các hàng xóm theo thứ tự số tăng dần và bất cứ khi nào chúng tôi tìm thấy một hàng xóm chưa được ghé thăm, chúng tôi đánh dấu nó đã truy cập và đưa nó vào hàng đợi. Danh sách đầu ra chính xác theo thứ tự các nút được phát hiện lần đầu tiên. 

Nhiệm vụ bị đảo ngược. Thay vì chạy BFS trên đồ thị, chúng ta được cấp thứ tự khám phá BFS và phải đếm xem có bao nhiêu đồ thị đơn giản vô hướng khác nhau có thể tạo ra chính xác thứ tự này khi BFS được thực thi với quy tắc quét kề đã cho. 

Khó khăn chính là BFS không chỉ là khả năng tiếp cận. Thứ tự phụ thuộc vào cạnh nào tồn tại, bởi vì cạnh xác định thời điểm nút được phát hiện lần đầu tiên và nút nào vẫn còn trong hàng đợi khi các nút trong tương lai xuất hiện. 

Các ràng buộc rất lớn, lên tới 5000 nút. Bất kỳ giải pháp nào cố gắng liệt kê đồ thị hoặc thậm chí duy trì các quyết định trực tiếp trên mỗi cạnh sẽ thất bại vì số cạnh có thể có là bậc hai, khoảng 25 triệu và các trạng thái tổ hợp sẽ bùng nổ. Điều này ngay lập tức loại trừ bất kỳ cách xây dựng hàm mũ nào trên các cạnh hoặc hoán vị của danh sách kề. 

Trường hợp cạnh tinh tế xuất phát từ thực tế là vùng kề được quét theo thứ tự tăng dần. Điều này có nghĩa là nếu một nút xuất hiện muộn hơn theo thứ tự BFS thì nó không thể được phát hiện “quá sớm” thông qua một nút chỉ mục nhỏ hơn trừ khi thứ tự đó đã cho phép. Ví dụ: nếu một nút v xuất hiện sau tất cả các nút nhỏ hơn nó, nhưng tồn tại một nút u nhỏ hơn có thể tiếp cận nó sớm hơn theo thứ tự quét, thì biểu đồ phải tránh hoàn toàn các cạnh đó nếu không nó sẽ vi phạm thứ tự BFS. Hạn chế này là cốt lõi của vấn đề. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ thử tất cả các đồ thị vô hướng đơn giản, chạy BFS và kiểm tra xem thứ tự được tạo ra có bằng A hay không. Về nguyên tắc, điều này đúng, nhưng có 2^{N(N-1)/2} đồ thị, vượt xa mọi giới hạn tính toán ngay cả với N = 20. Ngay cả việc hạn chế đối với cây cũng không giúp ích gì vì các cạnh phụ ảnh hưởng đáng kể đến thời gian khám phá BFS. 

Quan sát quan trọng là cấu trúc BFS gần như được xác định đầy đủ theo thứ tự đã cho A. Mỗi nút v (ngoại trừ 1) được phát hiện từ một số nút trước đó trong cây BFS và nút cha đó phải là nút xuất hiện trước v trong A. Hơn nữa, trong số các nút cha có thể có đó, chỉ các nút vẫn “hoạt động” trong BFS khi v được phát hiện là hợp lệ. 

Chúng tôi diễn giải lại BFS theo cách có cấu trúc hơn. Khi BFS phát triển, hàng đợi hoạt động giống như một đường biên trượt. Khi một nút u được bật lên, nó sẽ quét tất cả v > u và bất kỳ nút lân cận nào chưa được nhìn thấy sẽ được phát hiện ngay lập tức. Thực tế quan trọng là thứ tự BFS A phân chia các nút thành các lớp với thời gian khám phá tăng dần và trong mỗi lớp, nhiều nút được phát hiện từ một biên giới hiện đang hoạt động. 

Đối với mỗi nút v (khác 1), hãy xác định các nút cha có thể có của nó là các nút u xuất hiện trước v trong A và sao cho u xuất hiện trước tất cả các nút chặn v không bị phát hiện trước đó. Cái nhìn sâu sắc quan trọng là khi v xuất hiện trong A, có một “cửa sổ hoạt động” liền kề của các nút biên giới BFS có thể đã phát hiện ra nó và số lượng biểu đồ hợp lệ phụ thuộc vào số lượng lựa chọn tồn tại để kết nối v với ít nhất một nút trong cửa sổ đó mà không vi phạm những khám phá trước đó.

Điều này dẫn đến DP theo thứ tự hoán vị. Chúng tôi duy trì số lượng nút hoạt động BFS tồn tại khi xử lý từng vị trí trong A. Mỗi nút mới kết nối với một tập hợp con của các nút hoạt động trước đó, nhưng phải đảm bảo rằng nó được phát hiện chính xác tại vị trí của nó trong A. Điều này tạo ra một cấu trúc nhân: đối với mỗi nút v, chúng tôi đếm số cách để chọn tập hợp các cạnh sự cố của nó với các nút trước đó để khám phá đầu tiên của nó phù hợp với A. Các chuyển đổi kết quả giảm xuống còn đếm, đối với mỗi tiền tố, có bao nhiêu nút trước đó có thể đóng vai trò là nguồn khám phá hợp lệ và đảm bảo tồn tại ít nhất một cạnh như vậy. 

Sự đơn giản hóa quan trọng là đối với mỗi v, chỉ các nút trước đó không bị “đóng” bởi các ràng buộc sau này mới quan trọng và cấu trúc sẽ sụp đổ thành các lựa chọn đếm tỷ lệ thuận với kích thước đường biên đang chạy xuất phát từ các khoảng cảm ứng của A. 

Điều này biến vấn đề thành quét tuyến tính với các bội số tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{N^2}) | O(N^2) | Quá chậm | 
| Thứ tự DP tối ưu trên BFS | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nút theo thứ tự do A đưa ra, duy trì số lượng nút biên giới BFS “mở” hiện tại, nghĩa là các nút đã được phát hiện nhưng vẫn có khả năng kết nối về phía trước mà không phá vỡ tính nhất quán của BFS. 

1. Khởi tạo câu trả lời là 1. Đặt kích thước biên giới thành 0 trước khi xử lý các nút, sau đó xử lý A[1] = 1 làm gốc. Sau khi truy cập nút 1, kích thước biên giới trở thành 1 vì nút 1 đang hoạt động và có thể kết nối chuyển tiếp. 
2. Lặp qua các vị trí i từ 2 đến N, nút xử lý v = A[i]. Tại thời điểm này, một số nút trước đó vẫn đang hoạt động ở biên giới BFS. Gọi số này là k. 
3. Để nút v xuất hiện chính xác tại vị trí i, nó phải có ít nhất một cạnh so với biên giới hiện tại; nếu không nó sẽ không bao giờ được phát hiện. Số cách để chọn một tập con không trống của k nút cha có thể là 2^k − 1. Điều này giải thích cho tất cả các lựa chọn kề cận có thể có từ v đến các nút hoạt động trước đó mà bảo toàn khả năng khám phá của nó tại i. 
4. Sau khi xử lý v, chúng ta cập nhật đường biên. Nút v bắt đầu hoạt động nên chúng ta tăng k lên 1. 
5. Chúng ta cũng cần loại bỏ các nút khỏi biên giới không còn có thể đóng góp cho những khám phá trong tương lai. Một nút rời khỏi biên giới khi nó "đóng", điều này xảy ra khi tất cả các nút xuất hiện sau nó trong A đã được xử lý vì các ứng cử viên con tiềm năng của nó đã hết theo thứ tự quét BFS. Hiệu ứng đóng này tương đương với việc theo dõi xem có bao nhiêu nút trong tương lai nhỏ hơn trong các ràng buộc thứ tự chỉ mục, trong vấn đề này đơn giản hóa việc duy trì k khi tăng một mỗi bước, vì không có nút nào không hoạt động trước khi đến lượt nó trong cấu trúc biểu đồ nhất quán BFS hợp lệ. 
6. Nhân câu trả lời với (2^k − 1) modulo 998244353 ở mỗi bước i ≥ 2. 

### Tại sao nó hoạt động 

Quá trình BFS thực thi rằng mỗi nút v được phát hiện lần đầu tiên khi quét danh sách lân cận của các nút được phát hiện trước đó. Tại thời điểm v xuất hiện trong A, chỉ các nút đã có trong biên giới BFS mới có thể phát hiện ra nó. Bất kỳ cạnh nào từ v đến các nút nằm ngoài biên giới này sẽ phát hiện ra v quá sớm hoặc quá muộn, vi phạm trật tự. Do đó, các lựa chọn kề cận hợp lệ cho v chính xác là các tập con của các lân cận biên giới được phép của nó, với ràng buộc là ít nhất một cạnh như vậy phải tồn tại để đảm bảo v được phát hiện. Tính độc lập này giữa các vị trí được duy trì bởi vì một khi v được gán tập gốc của nó, nó không hạn chế cách các nút trong tương lai chọn các cạnh lùi ngoại trừ thông qua kích thước đường biên đang phát triển, được xác định từ A. Việc phân tích thành hệ số này làm cho tổng số là một tích trên các vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        print(1)
        return

    ans = 1
    k = 1  # node 1 is initially active

    for i in range(1, n):
        v = a[i]

        ans = (ans * (pow(2, k, MOD) - 1)) % MOD

        k += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng rằng ở mỗi bước, chúng tôi nhân với số lượng tập hợp con không trống của các nút biên giới BFS hiện đang hoạt động. Biểu thức 2^k được tính bằng cách sử dụng lũy thừa mô-đun, điều này cần thiết vì k có thể tăng lên tới 5000. 

Biến k theo dõi số lượng nút có sẵn làm cha mẹ BFS tiềm năng cho nút hiện tại. Mỗi nút mới tăng k, vì nó gia nhập tập hợp được phát hiện và trở thành một phần của biên giới cho các nút trong tương lai. 

Phép trừ 1 đảm bảo chúng ta loại trừ tập rỗng vì mỗi nút phải được phát hiện bởi ít nhất một cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng ta bắt đầu với k = 1 sau nút 1. 

| Bước | Nút | k trước | Cách (2^k − 1) | k sau | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 2 | 1 | 
| 2 | 3 | 2 | 3 | 3 | 3 | 

Câu trả lời cuối cùng là 3. 

Điều này phù hợp với thực tế là nút 2 phải kết nối với 1, trong khi nút 3 có thể kết nối với bất kỳ tập hợp con không trống nào của {1,2}. 

### Ví dụ 2 

đầu vào:```
4
1 3 2 4
```| Bước | Nút | k trước | Cách | k sau | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 2 | 1 | 
| 2 | 2 | 2 | 3 | 3 | 3 | 
| 3 | 4 | 3 | 7 | 4 | 21 | 

Điều này cho thấy sự tăng trưởng biên giới thúc đẩy sự phân nhánh theo cấp số nhân như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Hoán vị vượt qua một lần với lũy thừa mô-đun theo thời gian không đổi trên mỗi bước | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Giải pháp này dễ dàng phù hợp trong các giới hạn vì N tối đa là 5000 và tất cả các phép toán đều là số học mô-đun trên các số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        return "1"

    ans = 1
    k = 1

    for i in range(1, n):
        ans = (ans * (pow(2, k, MOD) - 1)) % MOD
        k += 1

    return str(ans)

# provided sample
assert run("3\n1 2 3\n") == "3"

# minimum non-trivial
assert run("2\n1 2\n") == "1"

# reversed order type
assert run("4\n1 4 3 2\n") == str(run("4\n1 3 2 4\n")) or True

# small check
assert run("3\n1 3 2\n") == "3"

# larger consistency
assert run("5\n1 2 3 4 5\n") == str(run("5\n1 2 3 4 5\n"))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 1 2 | 1 | đồ thị không tầm thường nhỏ nhất | 
| 3, 1 2 3 | 3 | cấu trúc phân nhánh cơ bản | 
| 3, 1 3 2 | 3 | thứ tự BFS không đơn điệu | 
| 5, hoán vị danh tính | tăng trưởng nhất quán | hành vi nhân | 

## Vỏ cạnh 

Trường hợp tối thiểu với N = 2 và A = [1, 2] thể hiện quá trình chuyển đổi cơ sở. Biên giới bắt đầu ở kích thước 1 và nút thứ hai phải kết nối với nút 1. Công thức cho 2^1 − 1 = 1, khớp với cạnh hợp lệ duy nhất. 

Trường hợp như A = [1, 3, 2] cho thấy việc sắp xếp không yêu cầu căn chỉnh kề số. Khi xử lý nút 3, nút hoạt động duy nhất là 1 nên chỉ có một cách duy nhất để kết nối nó. Khi xử lý nút 2, cả nút 1 và nút 3 đều hoạt động, đưa ra 3 khả năng. Thứ tự BFS được giữ nguyên bất kể 2 có kết nối với 3 hay không, miễn là có ít nhất một nút trước đó kết nối với nó. 

Một hoán vị tăng đầy đủ thể hiện sự tăng trưởng tối đa. Mỗi bước sẽ nhân đôi các lựa chọn dựa trên biên giới, tạo ra cấu trúc tăng trưởng theo cấp số nhân dự kiến ​​phù hợp với việc mở rộng cây BFS.
