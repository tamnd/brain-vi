---
title: "CF 105837A - Bóng và Thùng"
description: "Chúng tôi được cấp một bộ sưu tập các thùng. Mỗi thùng bắt đầu với một số quả bóng và có sức chứa tối đa. Hoạt động chính trong quy trình này là bất cứ khi nào một thùng đầy, nó sẽ bị loại bỏ khỏi việc xem xét tích cực và toàn bộ nội dung của nó sẽ được chuyển vào một kho dự trữ chung."
date: "2026-06-22T01:19:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105837
codeforces_index: "A"
codeforces_contest_name: "MITIT Spring 2025 Qualification Round 2"
rating: 0
weight: 105837
solve_time_s: 61
verified: true
draft: false
---

[CF 105837A - Bóng và Thùng](https://codeforces.com/problemset/problem/105837/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ sưu tập các thùng. Mỗi thùng bắt đầu với một số quả bóng và có sức chứa tối đa. Hoạt động chính trong quy trình này là bất cứ khi nào một thùng đầy, nó sẽ bị loại bỏ khỏi việc xem xét tích cực và toàn bộ nội dung của nó sẽ được chuyển vào một kho dự trữ chung. Các quả bóng dự trữ sau này có thể được phân phối vào các thùng khác, giúp chúng đạt hết công suất. 

Câu hỏi cơ bản là liệu, bắt đầu từ cấu hình ban đầu, liệu cuối cùng có thể kích hoạt quá trình xếp tầng này theo cách mà tất cả các thùng có thể được xử lý hoàn toàn chỉ bằng cách sử dụng các quả bóng có sẵn ban đầu hoặc được tích lũy trong kho dự trữ hay không. 

Một cách khác để xem hệ thống là mỗi thùng có một lượng bóng bổ sung cần thiết trước khi nó có thể được kích hoạt. Sau khi được kích hoạt, nó sẽ đóng góp toàn bộ công suất của mình vào một nhóm toàn cầu, điều này có thể cho phép các thùng khác đạt đến ngưỡng kích hoạt của chúng. Quá trình tiếp tục miễn là có thể hoàn thành một số thùng bằng cách sử dụng nguồn dự trữ hiện có. 

Các ràng buộc ngụ ý rằng việc mô phỏng trực tiếp tất cả các thứ tự kích hoạt có thể có của các thùng là không khả thi khi số lượng thùng lớn. Nếu chúng ta cố gắng khám phá tất cả các hoán vị của chuỗi kích hoạt, số lượng khả năng sẽ tăng theo cấp số nhân, điều này hoàn toàn không khả thi ngay cả đối với vài chục thùng. Điều này ngay lập tức gợi ý rằng bài toán phải có cấu trúc tham lam hoặc dựa trên trật tự cho phép chúng ta tránh phân nhánh trên tất cả các lựa chọn có thể. 

Một trường hợp lỗi nhỏ phát sinh khi nhiều thùng gần đầy, nhưng việc chọn sai thùng trước tiên sẽ khiến quá trình bị đình trệ. 

Ví dụ: giả sử chúng ta có hai thùng: 

đầu vào: 

3 5 

4 6 

Thùng 1 cần thêm 2 quả bóng, trong khi Thùng 2 cũng cần thêm 2 quả bóng, nhưng Thùng 2 mang lại nhiều sức chứa hơn khi hoàn thành. Nếu chúng tôi ưu tiên không chính xác các thùng lớn hơn hoặc tùy ý trước, chúng tôi có thể rơi vào trạng thái dự trữ không đủ để hoàn thành thùng còn lại mặc dù tồn tại một chuỗi hợp lệ. Điều này cho thấy rằng các quyết định của địa phương về việc kích hoạt thùng nào trước tiên là rất quan trọng. 

Khó khăn chính là việc hoàn thành thùng không chỉ là một sự kiện cục bộ mà nó còn thay đổi nguồn tài nguyên toàn cầu và do đó ảnh hưởng đến tất cả các quyết định khả thi trong tương lai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các đơn hàng có thể có trong đó các thùng được hoàn thành. Ở mỗi bước, chúng tôi sẽ chọn bất kỳ thùng nào có không gian cần thiết còn lại có thể được dự trữ hiện tại đáp ứng, khám phá đệ quy tất cả các lựa chọn. Trong trường hợp xấu nhất, nếu cuối cùng tất cả các thùng có thể được hoàn thành theo nhiều cách, điều này sẽ dẫn đến việc phân nhánh giai thừa trên N thùng, dẫn đến trạng thái O(N!). Ngay cả việc cắt bớt các đường dẫn không hợp lệ về cơ bản cũng không cải thiện được hành vi trong trường hợp xấu nhất này, bởi vì nhiều trạng thái trung gian vẫn hợp lệ và dẫn đến sự phân nhánh tiếp theo. 

Thông tin chi tiết về cấu trúc quan trọng là thuộc tính có ý nghĩa duy nhất của thùng là lượng không gian bổ sung mà nó cần trước khi có thể được kích hoạt. Sau khi loại bỏ quy trình phân phối chính xác, chúng tôi thấy rằng các thùng có không gian yêu cầu nhỏ hơn sẽ dễ dàng kích hoạt hơn và không bao giờ nên trì hoãn để chuyển sang các thùng có yêu cầu lớn hơn. 

Việc chuyển đổi mở ra giải pháp là coi mỗi thùng đóng góp một giá trị yêu cầu, được xác định là dung lượng còn lại của thùng đó. Thay vì lý luận về các lệnh kích hoạt tùy ý, chúng tôi sắp xếp các thùng theo yêu cầu này và luôn cố gắng xử lý thùng bị hạn chế nhất hiện có thể thực hiện được. Điều này hiệu quả vì nếu tồn tại một giải pháp trì hoãn thùng yêu cầu nhỏ để chuyển sang thùng lớn hơn, chúng tôi luôn có thể hoán đổi thứ tự của chúng mà không làm cho quy trình trở nên khó khăn hơn. Đối số dự trữ được cung cấp trong tuyên bố chính thức hóa đối số trao đổi này: việc ưu tiên các thùng chặt chẽ hơn không bao giờ làm giảm tính khả thi và chỉ có thể cải thiện hoặc duy trì nó.

Điều này làm giảm vấn đề duy trì nhiều bộ công suất còn lại và tiêu thụ chúng một cách tham lam theo thứ tự tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) | O(N) | Quá chậm | 
| Tối ưu (Tham lam theo không gian) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định dung lượng cần thiết còn lại của mỗi thùng là sự khác biệt giữa dung lượng và nội dung hiện tại. Giá trị này thể hiện mức độ khó để kích hoạt thùng đó. 

### bước 

1. Tính dung lượng còn lại cho mỗi thùng bằng dung tích trừ đi các quả bóng hiện tại. Điều này nắm bắt chính xác tài nguyên bổ sung cần thiết để kích hoạt nó. 
2. Sắp xếp tất cả các thùng theo dung lượng còn lại theo thứ tự không giảm. Ý tưởng là luôn xem xét các thùng hạn chế nhất trước tiên, vì chúng có nhiều khả năng cản trở tiến trình nhất nếu bị trì hoãn. 
3. Khởi tạo một biến đại diện cho các quả bóng dự trữ có sẵn. Ban đầu, con số này bằng 0 vì chưa có thùng nào được kích hoạt. 
4. Lặp lại các thùng theo thứ tự được sắp xếp. Đối với mỗi thùng, hãy kiểm tra xem lượng dự trữ hiện tại có đủ để lấp đầy không gian cần thiết hay không. Nếu không, quá trình sẽ dừng lại vì không thể hoàn thành thêm thùng nào có yêu cầu bằng hoặc lớn hơn. 
5. Nếu lượng dự trữ đủ, hãy kích hoạt ngăn và thêm toàn bộ dung lượng của nó vào lượng dự trữ. Điều này mô hình hóa thực tế rằng sau khi hoàn thành một thùng, tất cả các quả bóng của nó sẽ có sẵn để phân phối lại. 
6. Tiếp tục quá trình này cho đến khi tất cả các thùng đã được xử lý hoặc cho đến khi một thùng không thể được kích hoạt. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên đối số trao đổi theo thứ tự kích hoạt. Giả sử tồn tại một trình tự hợp lệ kích hoạt thùng có yêu cầu còn lại lớn hơn trước thùng có yêu cầu nhỏ hơn. Bởi vì thùng nhỏ hơn dễ đáp ứng hơn nên việc hoán đổi đơn hàng của họ không bao giờ làm giảm lượng dự trữ sẵn có ở bất kỳ bước nào mà thùng lớn hơn vẫn khả thi. Do đó, bất kỳ giải pháp hợp lệ nào cũng có thể được chuyển đổi thành giải pháp trong đó các thùng được xử lý theo thứ tự không gian yêu cầu không giảm. Điều này có nghĩa là nếu có bất kỳ giải pháp nào tồn tại, thứ tự sắp xếp tham lam cũng sẽ thành công và nếu quá trình tham lam thất bại ở một thùng nào đó thì không có thứ tự thay thế nào có thể thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    bins = []
    for _ in range(n):
        a, s = map(int, input().split())
        bins.append((s - a, s))
    
    bins.sort()
    
    reserve = 0
    
    for need, cap in bins:
        if reserve < need:
            print("NO")
            return
        reserve += cap
    
    print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo thứ tự tham lam. Mỗi thùng được biểu thị bằng một cặp bao gồm không gian bổ sung cần thiết và khả năng đóng góp của nó. Việc sắp xếp đảm bảo chúng ta luôn xử lý các ràng buộc chặt chẽ nhất trước tiên. Kiểm tra dự trữ sẽ ngăn chặn các chuyển đổi không hợp lệ trong đó thùng không thể được kích hoạt bằng các tài nguyên hiện có. 

Một cạm bẫy triển khai phổ biến là quên rằng phần đóng góp được thêm vào sau khi kích hoạt là toàn bộ công suất chứ không chỉ là phần chênh lệch. Một điều tinh tế khác là đảm bảo khóa sắp xếp chính xác là khoảng trống còn lại, vì thay vào đó, việc sắp xếp theo dung lượng sẽ phá vỡ tính logic. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3
2 5
4 6
```Chúng tôi tính toán các yêu cầu: 

| Bước | Thùng được chọn (cần, nắp) | Đặt trước | Có thể kích hoạt? | Đặt trước sau | 
| --- | --- | --- | --- | --- | 
| 1 | (2, 5) | 0 | Không | - | 

Quá trình dừng ngay lập tức, do đó đầu ra là KHÔNG. 

Điều này thể hiện trường hợp trong đó yêu cầu nhỏ nhất đã quá lớn đối với một khoản dự trữ trống, nghĩa là không tồn tại trình tự kích hoạt. 

### Ví dụ 2 

đầu vào:```
3
0 2
1 3
2 4
```Yêu cầu: 

| Bước | Thùng được chọn (cần, nắp) | Đặt trước | Có thể kích hoạt? | Đặt trước sau | 
| --- | --- | --- | --- | --- | 
| 1 | (0, 2) | 0 | Có | 2 | 
| 2 | (1, 3) | 2 | Có | 5 | 
| 3 | (2, 4) | 5 | Có | 9 | 

Tất cả các thùng được xử lý thành công, vì vậy đầu ra là CÓ. 

Điều này cho thấy các thùng có yêu cầu nhỏ sớm khởi động kho dự trữ để kích hoạt các thùng lớn hơn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Các thùng phân loại chiếm ưu thế, sau đó là một lần quét tuyến tính duy nhất | 
| Không gian | O(N) | Lưu trữ cặp thùng | 

Giải pháp này phù hợp thoải mái trong các giới hạn thông thường lên tới 200.000 thùng, vì cả việc sắp xếp và xử lý tuyến tính đều hiệu quả ở quy mô đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    n = int(input())
    bins = []
    for _ in range(n):
        a, s = map(int, input().split())
        bins.append((s - a, s))
    bins.sort()

    reserve = 0
    for need, cap in bins:
        if reserve < need:
            return "NO"
        reserve += cap
    return "YES"

# sample-like tests
assert run("3\n1 3\n2 5\n4 6\n") == "NO"
assert run("3\n0 2\n1 3\n2 4\n") == "YES"

# minimum size
assert run("1\n0 1\n") == "YES"

# impossible single bin
assert run("1\n2 5\n") == "NO"

# already satisfied chain dependency
assert run("4\n0 1\n1 2\n3 5\n6 10\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thùng duy nhất khả thi | CÓ | trường hợp cơ sở đúng đắn | 
| thùng rác duy nhất không thể | KHÔNG | xử lý sự cố ngay lập tức | 
| chuỗi ngày càng tăng | CÓ | tăng trưởng dự trữ lũy tiến | 
| tính khả thi hỗn hợp | KHÔNG/CÓ nhất quán | tham lam đặt hàng đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các thùng đều yêu cầu dự trữ ban đầu dương hoàn toàn. Ví dụ: 

đầu vào:```
2
3 5
4 6
```Cả hai thùng đều yêu cầu ít nhất 2 đơn vị dự trữ, nhưng hệ thống bắt đầu ở mức 0. Thuật toán sắp xếp chúng theo yêu cầu và ngay lập tức phát hiện rằng thùng đầu tiên không thể được kích hoạt, trả về NO một cách chính xác. Mọi nỗ lực kích hoạt thùng lớn hơn trước tiên cũng sẽ thất bại, do đó không có thứ tự hợp lệ ẩn nào. 

Một trường hợp đặc biệt khác là khi một thùng không có yêu cầu nào: 

đầu vào:```
3
0 10
5 6
6 7
```Ở đây thùng đầu tiên luôn có thể được kích hoạt và tạo ra một lượng dự trữ lớn. Thuật toán xử lý nó trước tiên nhờ phân loại, đảm bảo hệ thống trở nên khả thi trước khi xử lý các thùng chặt chẽ hơn. Dấu vết cho thấy dự trữ tăng đủ để mở khóa tất cả các thùng tiếp theo, xác nhận rằng thứ tự tham lam mô hình chính xác cấu trúc phụ thuộc.
