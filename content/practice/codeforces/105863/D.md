---
title: "CF 105863D - Đếm đồ thị tối thiểu"
description: "Chúng ta được cung cấp một cấu trúc đồ thị vô hướng được mô tả ngầm định thông qua các khoảng cách từ nút gốc cố định, nút 1. Đối với mỗi nút, khoảng cách của nó với nút 1 đều được biết và chúng ta được yêu cầu đếm xem có bao nhiêu đồ thị “tối thiểu” khác nhau có thể tạo ra chính xác cấu hình khoảng cách này."
date: "2026-06-22T02:13:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105863
codeforces_index: "D"
codeforces_contest_name: "PPSC 2025"
rating: 0
weight: 105863
solve_time_s: 48
verified: true
draft: false
---

[CF 105863D - Đếm đồ thị tối thiểu](https://codeforces.com/problemset/problem/105863/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc đồ thị vô hướng được mô tả ngầm định thông qua các khoảng cách từ nút gốc cố định, nút 1. Đối với mỗi nút, khoảng cách của nó với nút 1 đều được biết và chúng ta được yêu cầu đếm xem có bao nhiêu đồ thị “tối thiểu” khác nhau có thể tạo ra chính xác cấu hình khoảng cách này. 

Một biểu đồ được coi là hợp lệ nếu mọi nút ở khoảng cách d tính từ nút 1 được kết nối theo cách bảo toàn các khoảng cách đó. Đặc biệt, mọi nút phải có thể truy cập được thông qua một đường dẫn ngắn nhất, giảm khoảng cách nghiêm ngặt đi 1 ở mỗi bước cho đến khi đến nút 1. Hạn chế chính là tính tối thiểu, nghĩa là chúng ta không được phép thêm các cạnh bổ sung không đóng góp vào các đường dẫn ngắn nhất. 

Do đó, đầu vào mã hóa phân vùng các nút theo cấp độ BFS của chúng bắt đầu từ nút 1. Đầu ra là số cách để gán mối quan hệ cha mẹ qua các cấp độ liên tiếp sao cho tất cả các nút vẫn được kết nối nhất quán với khoảng cách nhất định của chúng. 

Nếu có n nút, một cách giải thích đơn giản sẽ gợi ý khám phá tất cả các cách có thể để kết nối các nút giữa các lớp trong khi vẫn duy trì các ràng buộc BFS. Điều đó sẽ ngay lập tức bùng nổ về mặt tổ hợp, vì mỗi nút có khả năng chọn nhiều nút cha ở lớp trước. Với n tối đa các giới hạn Codeforce điển hình như 10^5, mọi cách xây dựng hàm mũ hoặc thậm chí ghép bậc hai đều không khả thi. Điều này đẩy chúng ta tới một cấu trúc đếm nhân hoặc theo lớp. 

Một trường hợp lỗi tinh vi xuất hiện khi một số lớp khoảng cách tồn tại mà không có lớp trước đó. Ví dụ: nếu có một nút ở khoảng cách 2 nhưng không có nút nào ở khoảng cách 1 thì không thể tồn tại đồ thị hợp lệ vì nút đó sẽ không có cách nào để kết nối với gốc thông qua các đường đi ngắn nhất. Trong những trường hợp như vậy, câu trả lời phải bằng 0. Một trường hợp khác là khi nhiều nút có cùng khoảng cách, vì các lựa chọn của chúng không độc lập theo nghĩa đơn giản mà trở nên độc lập khi chúng ta nhận ra cấu trúc của lựa chọn cha hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng tất cả các cấu trúc khung hợp lệ phù hợp với việc dán nhãn khoảng cách nhất định. Đối với mỗi nút ở khoảng cách d, chúng tôi sẽ thử mọi nút cha có thể có trong số các nút ở khoảng cách d − 1, đảm bảo đệ quy các ràng buộc kết nối. Điều này ngay lập tức trở thành cấp số nhân: nếu một cấp độ có k nút và cấp độ trước đó có m nút, thì có thể có m^k nhiệm vụ chỉ dành cho lớp đó. Ngay cả đối với những giá trị khiêm tốn như m = 5 và k = 20, con số này đã lớn về mặt thiên văn rồi. 

Quan sát quan trọng là ràng buộc khoảng cách định vị hoàn toàn các lựa chọn giữa các lớp liền kề. Một nút ở khoảng cách d không thể kết nối với bất cứ thứ gì ngoại trừ các nút ở khoảng cách d - 1 nếu chúng ta muốn mức tối thiểu, bởi vì bất kỳ cạnh nào khác sẽ vi phạm cấu trúc đường đi ngắn nhất hoặc dư thừa. Điều này làm giảm toàn bộ vấn đề xây dựng đồ thị thành các lựa chọn độc lập cho mỗi lớp. 

Bây giờ cấu trúc trở nên đơn giản: xử lý các nút được nhóm theo khoảng cách từ 1. Giả sử chúng ta đang xem xét một số lớp d. Mỗi nút trong lớp này phải chọn chính xác một nút cha trong lớp d − 1. Nếu có x nút trong lớp d − 1 thì mỗi nút trong lớp d có x lựa chọn độc lập. Tuy nhiên, chúng tôi đang tính các cấu trúc toàn cầu hợp lệ, vì vậy thay vì xử lý các nút một cách độc lập giữa các lớp, chúng tôi tích lũy theo cấp số nhân trên mỗi lớp. Hạn chế toàn cầu duy nhất là mọi lớp không phải gốc phải có ít nhất một nút ở lớp trước đó; nếu không thì không thể xây dựng được.

Điều này biến vấn đề thành sắp xếp các nút theo khoảng cách, đếm số lượng xuất hiện trong mỗi lớp và nhân các đóng góp của biểu mẫu (số lượng nút ở lớp trước) được nâng lên một cách ngầm định thông qua các lựa chọn độc lập lặp đi lặp lại. Bởi vì mỗi nút chọn nút cha một cách độc lập nên mức đóng góp của một lớp là x^(k_d), nhưng do tuyên bố biên tập đơn giản hóa nó thành một sự tích lũy tuần tự trên mỗi thứ tự nút, nên chúng ta cũng có thể hiểu nó là các nút lặp theo thứ tự khoảng cách tăng dần và nhân với kích thước của lớp trước đó khi gặp ranh giới nhóm khoảng cách mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng cách nút và nhóm các nút theo khoảng cách của chúng với nút 1. Sắp xếp hoặc đếm tần số theo mức khoảng cách là đủ vì chỉ có sự phân lớp tương đối mới quan trọng chứ không phải danh tính. Điều này đảm bảo chúng ta có thể xử lý từng lớp biểu đồ. 
2. Sắp xếp các giá trị khoảng cách khác nhau theo thứ tự tăng dần. Điều này thiết lập thứ tự phân lớp BFS bắt đầu từ khoảng cách 0 tại nút 1. 
3. Khởi tạo biến trả lời là 1 và duy trì biến prev, biểu thị số nút ở lớp trước. Bắt đầu với prev = 1 vì nút 1 là nút duy nhất ở khoảng cách 0. 
4. Lặp lại từng lớp khoảng cách d theo thứ tự tăng dần. Gọi cur là số nút ở khoảng cách này. Nếu d không chính xác là prev_distance + 1 về sự tồn tại của các lớp (có nghĩa là có khoảng cách về khoảng cách), ngay lập tức trả về 0. Điều này thực thi kết nối thông qua các lớp BFS liên tiếp. 
5. Đối với mỗi lớp hợp lệ, hãy nhân câu trả lời với kết quả trước đó được nêu trong mô hình khái niệm. Theo cách giải thích đơn giản được sử dụng trong báo cáo vấn đề, điều này được xử lý dưới dạng nhân lặp lại với trước cho mỗi nút trong lớp. Sau đó cập nhật prev = cur cho lần lặp tiếp theo. 
6. Trả về câu trả lời tích lũy cuối cùng. 

Tính chính xác dựa trên tính bất biến là sau khi xử lý lớp d, tất cả các nút trong lớp ≤ d đã được gán các kết nối gốc hợp lệ phù hợp với các ràng buộc đường dẫn ngắn nhất và số lượng lựa chọn có sẵn cho lớp tiếp theo chỉ phụ thuộc vào kích thước của lớp hiện tại, không phụ thuộc vào cấu trúc bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    dist = list(map(int, input().split()))

    freq = {}
    for d in dist:
        freq[d] = freq.get(d, 0) + 1

    layers = sorted(freq.items())

    # must start from distance 0 (node 1)
    if layers[0][0] != 0:
        print(0)
        return

    ans = 1
    prev = layers[0][1]  # nodes at distance 0, should be 1

    for i in range(1, len(layers)):
        d, cur = layers[i]

        # distance must be consecutive
        if layers[i - 1][0] + 1 != d:
            print(0)
            return

        # each node chooses a parent in previous layer
        ans = (ans * pow(prev, cur, MOD)) % MOD

        prev = cur

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén đầu vào thành số lượng tần số trên mỗi mức khoảng cách. Điều này loại bỏ độ phức tạp ở cấp độ nhận dạng và chỉ để lại thông tin cấu trúc. 

Việc sắp xếp`layers`list đảm bảo chúng tôi xử lý các cấp độ BFS theo thứ tự. Việc kiểm tra các khoảng cách liên tiếp buộc mọi lớp phải có thể truy cập được từ lớp trước đó; nếu không thì không thể tồn tại đồ thị tối thiểu. 

Quá trình chuyển đổi quan trọng là`ans = ans * pow(prev, cur, MOD)`, mã hóa rằng mỗi`cur`các nút độc lập chọn một trong`prev`cha mẹ. Phép lũy thừa mô đun là cần thiết bởi vì`cur`có thể lớn và phép nhân lặp lại sẽ quá chậm. 

Biến`prev`được cập nhật thành`cur`sau khi xử lý từng lớp, duy trì tính bất biến là nó luôn thể hiện kích thước của cấp BFS trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét khoảng cách:`[0, 1, 1, 2]`| Bước | Lớp | trước | cur | Đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | ban đầu | 1 | 
| 2 | 1 | 1 | 2 | 1^2 = 1 | 1 | 
| 3 | 2 | 2 | 1 | 2^1 = 2 | 2 | 

Điều này cho thấy rằng hai nút ở khoảng cách 1, mỗi nút có một lựa chọn cha mẹ duy nhất (nút 1) và nút ở khoảng cách 2 có hai lựa chọn trong số lớp trước đó. 

Câu trả lời cuối cùng là 2. 

### Ví dụ 2 

Hãy xem xét khoảng cách:`[0, 1, 2, 2]`| Bước | Lớp | trước | cur | Đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | ban đầu | 1 | 
| 2 | 1 | 1 | 1 | 1^1 = 1 | 1 | 
| 3 | 2 | 1 | 2 | 1^2 = 1 | 1 | 

Mặc dù có hai nút ở khoảng cách 2, nhưng mỗi nút có chính xác một lựa chọn cha, vì vậy câu trả lời vẫn là 1. 

Những dấu vết này xác nhận rằng việc tính toán chỉ phụ thuộc vào kích thước lớp chứ không phụ thuộc vào cấu trúc bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | nhóm khoảng cách sắp xếp chiếm ưu thế | 
| Không gian | O(n) | lưu trữ tần số khoảng cách | 

Thuật toán dễ dàng phù hợp với các ràng buộc điển hình cho n tối đa 10^5 hoặc cao hơn. Sắp xếp là thành phần phi tuyến tính duy nhất và tất cả các hoạt động khác là quét tuyến tính trên các lớp nén. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    n = int(input())
    dist = list(map(int, input().split()))

    freq = {}
    for d in dist:
        freq[d] = freq.get(d, 0) + 1

    layers = sorted(freq.items())

    if layers[0][0] != 0:
        return "0\n"

    ans = 1
    prev = layers[0][1]

    for i in range(1, len(layers)):
        d, cur = layers[i]
        if layers[i - 1][0] + 1 != d:
            return "0\n"
        ans = (ans * pow(prev, cur, MOD)) % MOD
        prev = cur

    return str(ans) + "\n"

# sample-like tests
assert run("4\n0 1 1 2\n") == "2\n"
assert run("4\n0 1 2 2\n") == "1\n"

# custom cases
assert run("1\n0\n") == "1\n"
assert run("2\n0 2\n") == "0\n"
assert run("3\n0 1 2\n") == "1\n"
assert run("5\n0 1 1 1 2\n") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ 1 nút | 1 | trường hợp tối thiểu | 
| thiếu lớp 1 | 0 | phát hiện khoảng cách | 
| chuỗi nghiêm ngặt | 1 | cấu trúc đường dẫn đơn | 
| nhiều nút trên mỗi lớp | 3 | phân nhánh nhân | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi lớp đầu tiên sau khoảng cách 0 bị thiếu hoặc bị lệch. Đối với đầu vào như`0 2`, có một nút ở khoảng cách 2 nhưng không có nút nào ở khoảng cách 1. Thuật toán phát hiện điều này khi kiểm tra sự khác biệt của các lớp liên tiếp và ngay lập tức trả về 0 trước khi bất kỳ phép nhân nào xảy ra. 

Một trường hợp khác là biểu đồ nút đơn. Với đầu vào`[0]`, bản đồ tần số chỉ tạo ra một lớp. Thuật toán khởi tạo`ans = 1`và không bao giờ vào vòng lặp, trả về chính xác 1 vì có chính xác một biểu đồ tầm thường. 

Trường hợp thứ ba là khi các lớp tồn tại nhưng không liên tục, chẳng hạn như`[0, 1, 1, 3]`. Khi xử lý lớp 3, lớp trước đó là 1 và khoảng cách tiếp theo dự kiến ​​​​sẽ là 2. Việc kiểm tra không thành công và trả về 0, ngăn chặn mọi tính toán từng phần không hợp lệ.
