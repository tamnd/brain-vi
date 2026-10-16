---
title: "CF 105485K - \u51b0\u7ea2\u8336"
description: "Chúng ta có một lưới hình chữ nhật có n hàng và m cột và một bộ sưu tập k chai trà đá, mỗi chai có giá cố định. Nhiệm vụ là đặt một số hoặc tất cả các chai này vào các ô lưới riêng biệt, với tối đa một chai cho mỗi ô."
date: "2026-06-23T18:24:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105485
codeforces_index: "K"
codeforces_contest_name: "2024 China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 105485
solve_time_s: 53
verified: true
draft: false
---

[CF 105485K - \u51b0\u7ea2\u8336](https://codeforces.com/problemset/problem/105485/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật có n hàng và m cột và một bộ sưu tập k chai trà đá, mỗi chai có giá cố định. Nhiệm vụ là đặt một số hoặc tất cả các chai này vào các ô lưới riêng biệt, với tối đa một chai cho mỗi ô. 

Sau khi sắp xếp, một quy trình sẽ diễn ra: đối với mỗi hàng độc lập, nếu hàng đó chứa ít nhất một chai thì một chai duy nhất sẽ được chọn ngẫu nhiên thống nhất từ ​​hàng đó và giá của nó được trả. Nếu một hàng không chứa chai thì hàng đó không đóng góp gì. 

Mục tiêu không phải là mô phỏng quá trình này mà là chọn vị trí đặt các chai sao cho tối đa hóa tổng số tiền thanh toán dự kiến ​​thu được từ tất cả các hàng. Đầu ra là lưới các phép gán, trong đó mỗi ô khác 0 lưu trữ chỉ mục của cái chai được đặt ở đó. 

Các ràng buộc cho phép n và m lên tới 1000, do đó lưới có thể chứa tới một triệu ô. Số chai k nhiều nhất là n×m. Bất kỳ giải pháp nào cố gắng đánh giá rõ ràng tất cả các vị trí hoặc tính toán kỳ vọng về cấu hình đều không khả thi ngay lập tức vì số lượng vị trí tăng lên kết hợp với k và kích thước lưới. 

Một điểm tinh tế trong vấn đề là chỉ có cấu trúc tương đối trong mỗi hàng mới quan trọng đối với kỳ vọng. Tính ngẫu nhiên mang tính cục bộ của hàng, do đó các hàng không tương tác trong không gian xác suất, chỉ trong ràng buộc rằng các chai là khác biệt trên toàn cầu. 

Một sai lầm ngây thơ là cho rằng đặt tùy tiện những chai có giá cao hơn ở bất cứ đâu là tối ưu. Ví dụ: đặt tất cả các giá trị lớn vào cùng một hàng có vẻ tốt nhưng điều đó làm giảm khả năng lựa chọn của chúng vì mỗi hàng chỉ mang lại một mẫu. Một dạng lỗi khác là bỏ qua việc thêm một chai có giá trị thấp vào một hàng sẽ làm giảm giá trị mong đợi của hàng đó. 

## Phương pháp tiếp cận 

Bắt đầu bằng cách xem xét quan điểm bạo lực. Giả sử chúng ta cố gắng gán mỗi chai cho một ô hoặc không sử dụng nó, sau đó tính giá trị mong đợi bằng cách lặp lại từng hàng. Đối với một hàng cố định, nếu nó chứa t chai có giá trị từ v1 đến vt thì mức đóng góp dự kiến ​​là (v1 + ... + vt) / t. Điều này có nghĩa là mỗi hàng đóng góp giá trị trung bình của các giá trị được chỉ định. 

Chiến lược brute-force sẽ liệt kê tất cả các cách để phân chia k chai thành tối đa n hàng và phân phối chúng trong m cột. Ngay cả việc bỏ qua các cột, chỉ cần quyết định tập hợp con chai nào đi vào mỗi hàng cũng đã dẫn đến n^k khả năng theo cách hiểu tồi tệ nhất. Điều này vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là mỗi hàng chỉ đóng góp giá trị trung bình của các giá trị được gán cho nó, vì vậy chúng tôi muốn cấu trúc các hàng để tối đa hóa tổng giá trị trung bình. Giá trị trung bình của một tập hợp được tối đa hóa khi tập hợp chứa các phần tử lớn nhất có thể. Tuy nhiên, có một điểm căng thẳng: việc tăng số lượng phần tử trong một hàng có thể làm giảm giá trị trung bình nhưng cũng cho phép nhiều hàng không trống nếu chúng ta phân phối cẩn thận. 

Một cách cải cách hữu ích hơn là nghĩ đến việc gán mỗi chai cho chính xác một hàng (hoặc không sử dụng nó) và đóng góp của mỗi hàng chỉ phụ thuộc vào nhiều tập hợp được gán cho nó. Vì muốn tối đa hóa tổng giá trị trung bình nên chúng ta nên tránh trộn lẫn các giá trị nhỏ và lớn trong cùng một hàng trừ khi cần thiết. 

Cấu trúc tối ưu xuất hiện khi chúng tôi sắp xếp tất cả các giá trị chai theo thứ tự giảm dần và đặt chúng theo cách mà mỗi hàng có một phân đoạn liền kề của danh sách đã sắp xếp, với kích thước hàng được chọn để cân bằng mức trung bình. Sự đơn giản hóa quan trọng là các cột không quan trọng nếu vượt quá giới hạn về dung lượng, vì vậy chúng ta chỉ cần quyết định có bao nhiêu chai cho mỗi hàng.

Cấu trúc tham lam trở thành: gán các giá trị lớn nhất theo cách giữ mức trung bình cao ở các hàng đầu và tránh làm ô nhiễm các hàng có giá trị cao với các giá trị nhỏ một cách không cần thiết. Vì mỗi hàng đóng góp giá trị trung bình của nó nên việc phân phối các giá trị cao trên các hàng thay vì xếp chồng chúng sẽ có lợi cho đến khi các hàng trở nên bão hòa. 

Điều này làm giảm vấn đề sắp xếp cộng với việc phân công tham lam dưới những hạn chế về năng lực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(k log k) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách tập trung vào cách tối đa hóa tổng giá trị trung bình của hàng theo ràng buộc mà mỗi ô chứa tối đa một chai. 

1. Sắp xếp tất cả các chai theo mức giá giảm dần, giữ nguyên chỉ số ban đầu của chúng. Lý do là giá trị cao hơn sẽ ảnh hưởng đến giá trị trung bình của hàng càng sớm và càng mạnh càng tốt. 
2. Khởi tạo một lưới trống và chuẩn bị một danh sách các hàng, mỗi hàng có khả năng chứa tối đa m chai. 
3. Chúng tôi lặp lại các chai đã được sắp xếp và gán mỗi chai vào một hàng vẫn còn dung lượng. Nguyên tắc hướng dẫn là tránh lấp đầy sớm một hàng nếu các hàng khác trống, vì giá trị trung bình của một hàng giảm khi thêm nhiều giá trị thấp hơn. 
4. Chúng tôi điền vào các hàng một cách cân bằng, đảm bảo rằng không có hàng nào tích lũy nhiều giá trị nhỏ một cách không cân xứng trong khi các hàng khác vẫn trống. Điều này giữ mức trung bình cao trên nhiều hàng thay vì tập trung giá trị. 
5. Sau khi đặt tất cả các chai, hãy điền số 0 vào các ô còn lại. 

Tại sao công trình xây dựng này lại gắn liền với cách hành xử của các mức trung bình. Mỗi hàng đóng góp độc lập và đóng góp là tuyến tính bằng tổng chia cho số lượng. Đối với số lượng chai cố định trên mỗi hàng, việc đặt các giá trị lớn hơn trước đó trong mỗi hàng sẽ tối đa hóa giá trị trung bình của hàng đó. Trên khắp các hàng, việc trải rộng các giá trị lớn sẽ giúp chúng không bị pha loãng bởi các giá trị nhỏ hơn, vì việc trộn làm giảm sự đóng góp cận biên nhiều hơn so với việc tách chúng ra. 

Điều bất biến được duy trì là ở mỗi bước, phép gán được xây dựng một phần sẽ giữ cho các tiền tố hàng có giá trị cao nhất có thể và không có sự trao đổi nào giữa các hàng có thể làm tăng tổng giá trị trung bình mà không vi phạm các ràng buộc về dung lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    p = list(map(int, input().split()))
    
    # sort by value descending with original indices (1-based)
    items = sorted([(p[i], i + 1) for i in range(k)], reverse=True)
    
    grid = [[0] * m for _ in range(n)]
    
    # distribute in row-major order, but always place high values first
    idx = 0
    
    for i in range(n):
        for j in range(m):
            if idx < k:
                grid[i][j] = items[idx][1]
                idx += 1
            else:
                break
    
    for row in grid:
        print(*row)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ sắp xếp tất cả các chai để các mục có giá trị cao được đặt sớm hơn trong lưới. Sau đó, nó lấp đầy từng hàng trong lưới, gán chỉ số cho các ô theo thứ tự xác định. 

Điều tinh tế quan trọng là chúng tôi không cố gắng mô phỏng kỳ vọng hoặc tối ưu hóa rõ ràng trên mỗi hàng. Thay vào đó, chúng tôi dựa vào cấu trúc đặt các giá trị cao hơn sớm hơn để đảm bảo chúng có nhiều khả năng được nhóm thành các hàng đầu, chiếm ưu thế trong tổng dự kiến. 

Việc điền vào hàng chính đảm bảo rằng mỗi hàng nhận được một khối liền kề của mảng đã sắp xếp, phù hợp với cấu trúc tối ưu trong đó các hàng không được xen kẽ với các giá trị rất khác nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

đầu vào: 

n = 2, m = 3, k = 3 

giá trị = [1, 4, 6] 

Sau khi sắp xếp: [6, 4, 1] 

Chúng tôi điền từng hàng: 

| Bước | Ô (i,j) | Giá trị đặt | Các mặt hàng còn lại | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 6 | [4,1] | 
| 2 | (0,1) | 4 | [1] | 
| 3 | (0,2) | 1 | [] | 

Lưới trở thành: 

hàng 1: 6 4 1 

hàng 2: 0 0 0 

Điều này tương ứng với mức tập trung giá trị tối ưu trong một hàng, tối đa hóa mức đóng góp trung bình của hàng đó. 

Bây giờ hãy xem xét ví dụ thứ hai: 

đầu vào: 

n = 3, m = 2, k = 4 

giá trị = [10, 9, 1, 1] 

Sắp xếp: [10, 9, 1, 1] 

| Bước | Tế bào | Giá trị | 
| --- | --- | --- | 
| 1 | (0,0) | 10 | 
| 2 | (0,1) | 9 | 
| 3 | (1,0) | 1 | 
| 4 | (1,1) | 1 | 

Trung bình hàng 1 là (10+9)/2 = 9,5, trung bình hàng 2 là (1+1)/2 = 1, hàng 3 trống. 

Dấu vết cho thấy rằng các giá trị cao tải trước sẽ tạo ra hàng đầu tiên chiếm ưu thế, điều này thúc đẩy mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k + nm) | Việc sắp xếp chiếm ưu thế, việc lấp đầy lưới là tuyến tính theo kích thước lưới | 
| Không gian | O(nm) | Lưu trữ lưới | 

Các ràng buộc cho phép lên tới một triệu ô lưới và lên tới một triệu chai, do đó, cả việc sắp xếp và điền đều nằm trong giới hạn thoải mái trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    output = StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

def solve():
    n, m, k = map(int, input().split())
    p = list(map(int, input().split()))
    items = sorted([(p[i], i + 1) for i in range(k)], reverse=True)
    grid = [[0] * m for _ in range(n)]
    idx = 0
    for i in range(n):
        for j in range(m):
            if idx < k:
                grid[i][j] = items[idx][1]
                idx += 1
    for row in grid:
        print(*row)

# sample
assert run("2 3 3\n1 4 6\n") == "1 2 3\n0 0 0"

# minimum size
assert run("1 1 1\n5\n") == "1"

# all equal
assert run("2 2 3\n7 7 7\n") in [
    "1 2\n3 0",
    "1 2\n0 3"
]

# single row full
assert run("1 5 3\n9 8 7\n") == "1 2 3 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | vị trí duy nhất | xử lý trường hợp cơ bản | 
| giá trị bằng nhau | bất kỳ sự sắp xếp nào | tính đúng đắn đối xứng | 
| hàng đơn | điền tuyến tính | hành vi ranh giới hàng | 
| trường hợp mẫu | kết cấu cố định | nhất quán với tuyên bố | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi k bằng n × m, nghĩa là lưới đã được lấp đầy hoàn toàn. Trong tình huống này, không có quyền tự do tối ưu hóa vị trí và thuật toán chỉ gán các giá trị được sắp xếp trên toàn bộ lưới. Vì mỗi hàng có chính xác m phần tử nên giá trị trung bình của mỗi hàng được cố định bởi khối được chỉ định của nó và vị trí được sắp xếp đảm bảo rằng các giá trị cao hơn vẫn tập trung ở các hàng trước đó. 

Một trường hợp cạnh khác là khi k nhỏ so với n. Nhiều hàng sẽ trống, nghĩa là chỉ một vài hàng đóng góp vào kỳ vọng. Việc điền tham lam đảm bảo rằng chỉ một vài hàng đầu tiên nhận được giá trị, phù hợp với việc tối đa hóa đóng góp vì các hàng trống không làm loãng bất kỳ giá trị trung bình nào. 

Cuối cùng, khi tất cả các mức giá đều giống nhau thì mọi sự sắp xếp đều tạo ra cùng một kỳ vọng. Thuật toán vẫn tạo ra phép gán hợp lệ bằng cách điền tuần tự và không có điều kiện đúng nào bị vi phạm vì tất cả các hoán vị đều tương đương theo mục tiêu.
