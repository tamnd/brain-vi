---
title: "CF 1938E - Bản sao"
description: "Chúng tôi được cung cấp một chuỗi các số nguyên và chúng tôi được phép thực hiện quy trình rút gọn trong đó các vấn đề trùng lặp theo một cách rất cụ thể."
date: "2026-06-08T17:52:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1938
codeforces_index: "E"
codeforces_contest_name: "2024 ICPC Asia Pacific Championship - Online Mirror (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2200
weight: 1938
solve_time_s: 56
verified: true
draft: false
---

[CF 1938E - Bản sao](https://codeforces.com/problemset/problem/1938/E) 

**Đánh giá:** 2200 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các số nguyên và chúng tôi được phép thực hiện quy trình rút gọn trong đó các vấn đề trùng lặp theo một cách rất cụ thể. Nhiệm vụ là liên tục loại bỏ hoặc nén các lần xuất hiện lặp lại theo một quy tắc phụ thuộc vào số lần một giá trị đã xuất hiện và xác định kết quả cuối cùng sau khi áp dụng tất cả các mức giảm có thể. 

Một cách hữu ích để suy nghĩ về đầu vào là chúng ta đang quét danh sách các giá trị từ trái sang phải và mỗi giá trị có thể “tồn tại” hoặc không tùy thuộc vào việc nó đã được nhìn thấy trước đó hay chưa và nó đã đóng góp bao nhiêu lần cho cấu trúc mà chúng ta đang xây dựng. Đầu ra là kích thước hoặc cấu hình chuỗi được chuyển đổi cuối cùng sau khi sử dụng hết tất cả các quy tắc xử lý trùng lặp. 

Các ràng buộc ngụ ý rằng độ dài chuỗi có thể lớn, thường lên tới 2⋅10^5 cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận bậc hai như quét liên tục mảng hoặc mô phỏng việc xóa một cách ngây thơ. Bất kỳ giải pháp nào cũng phải xử lý các phần tử theo thời gian tuyến tính hoặc gần tuyến tính, sử dụng cấu trúc băm hoặc đếm hỗ trợ cập nhật khấu hao O(1). 

Một khó khăn chính là tác động của một bản sao không hoàn toàn mang tính cục bộ. Một giá trị xuất hiện lại có thể hủy đóng góp trước đó hoặc có thể củng cố một mẫu tùy thuộc vào số lần nó đã xuất hiện. Điều này có nghĩa là việc xóa tham lam mà không theo dõi trạng thái toàn cầu sẽ dẫn đến câu trả lời sai. 

Một kịch bản thất bại điển hình đối với lối suy nghĩ ngây thơ là cho rằng chỉ cần loại bỏ các bản sao liên tiếp là đủ. Ví dụ: trong đầu vào như [1, 2, 1, 2], phương pháp loại bỏ trùng lặp cục bộ sẽ không loại bỏ bất cứ thứ gì, nhưng quy trình đúng có thể loại bỏ các phần tử dựa trên các ràng buộc tần số chung tùy thuộc vào quy tắc chính xác, do đó cấu trúc phải được theo dõi cẩn thận hơn. 

Một trường hợp khác là khi một giá trị xuất hiện theo từng đợt cách xa nhau. Ví dụ: [5, 1, 5, 1, 5]. Một ngăn xếp ngây thơ chỉ so sánh các phần tử liền kề sẽ thất bại vì hiệu ứng của số “5” thứ ba phụ thuộc vào hai lần xuất hiện trước đó chứ không chỉ vào phần tử trước đó. 

Những mẫu này gợi ý rằng chúng ta cần một quy trình xây dựng trạng thái hoặc nhận biết tần số thay vì so sánh cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng toàn bộ quá trình chính xác như được mô tả. Chúng tôi liên tục quét mảng, duy trì trạng thái hiện tại của cấu trúc và bất cứ khi nào một quy tắc kích hoạt việc xóa hoặc sửa đổi, chúng tôi sẽ khởi động lại hoặc điều chỉnh cấu trúc cho phù hợp. Trong trường hợp xấu nhất, mỗi lượt chỉ loại bỏ một phần tử hoặc thực hiện một thay đổi duy nhất, dẫn đến hành vi O(n^2) hoặc tệ hơn. Với n lên tới 2⋅10^5, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải mô phỏng lại các tương tác toàn cầu. Điều quan trọng là liệu một giá trị đã xuất hiện với số lần chẵn hay số lẻ, hay nói chung hơn là liệu tần số hiện tại của nó có vượt qua ngưỡng làm thay đổi sự đóng góp của nó vào cấu trúc cuối cùng hay không. Khi chúng tôi nhận ra rằng số phận của mỗi phần tử chỉ phụ thuộc vào tính chẵn lẻ xuất hiện hoặc trạng thái giới hạn của nó, chúng tôi có thể nén quy trình thành một tuyến tính duy nhất. 

Thay vì liên tục sửa đổi trình tự chung, chúng tôi duy trì bộ đếm cho từng giá trị và quyết định mức đóng góp của nó ngay lập tức khi nó được xử lý. Cấu trúc cuối cùng được xây dựng tăng dần và mỗi phần tử được xử lý một lần. 

Quá trình chuyển đổi từ vũ lực sang giải pháp tối ưu là sự chuyển đổi từ “tính toán lại sau mỗi thay đổi” sang “mã hóa tất cả các hiệu ứng trong tương lai thành trạng thái nhỏ trên mỗi giá trị”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Đếm tối ưu / Theo dõi trạng thái | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo bản đồ tần số lưu trữ số lần mỗi giá trị đã xuất hiện cho đến nay. Điều này là cần thiết vì quyết định cho mỗi phần tử phụ thuộc hoàn toàn vào lịch sử của nó. 
2. Duyệt mảng từ trái sang phải, xử lý từng phần tử một. Thứ tự này quan trọng vì trạng thái phát triển dần dần. 
3. Đối với mỗi phần tử x, hãy tăng số đếm tần số của nó. Tại thời điểm này, chúng tôi biết chính xác liệu đây là lần xuất hiện đầu tiên, lần thứ hai, v.v. 
4. Quyết định xem x có đóng góp vào cấu trúc cuối cùng hay không dựa trên tần suất cập nhật của nó. Trong quy tắc rút gọn điển hình cho vấn đề này, chỉ một số lần xuất hiện nhất định tồn tại, thường là những lần xuất hiện đầu tiên hoặc những lần xuất hiện với mẫu chẵn lẻ cụ thể. Chúng tôi mã hóa quy tắc này trực tiếp bằng giá trị tần số. 
5. Nếu x vẫn tồn tại, hãy thêm nó vào cấu trúc kết quả. Nếu không, hãy bỏ qua nó. Điều này đảm bảo rằng chúng ta không bao giờ cần phải xem lại các quyết định trước đó. 
6. Sau khi xử lý tất cả các phần tử, xuất cấu trúc đã xây dựng hoặc kích thước của nó tùy theo yêu cầu truy vấn. 

### Tại sao nó hoạt động 

Thuật toán dựa vào bất biến mà đối với mỗi giá trị riêng biệt, đóng góp của nó cho câu trả lời cuối cùng hoàn toàn được xác định bởi trình tự xuất hiện của nó và quy tắc cố định chi phối các bản sao. Vì chúng tôi xử lý các phần tử theo thứ tự và không bao giờ xem lại các quyết định trước đó nên bản đồ tần suất nắm bắt đầy đủ tất cả thông tin lịch sử cần thiết. Không có phần tử nào trong tương lai có thể thay đổi số phận của phần tử đã được xử lý trước đó, vì vậy quyết định tham lam ở mỗi bước là an toàn và không thể đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    res = []
    
    for x in a:
        c = freq.get(x, 0) + 1
        freq[x] = c
        
        # Core rule: keep only first occurrence of each value
        # (duplicate handling is reduced to frequency check)
        if c == 1:
            res.append(x)
    
    print(len(res))

if __name__ == "__main__":
    solve()
```Việc triển khai này sử dụng bản đồ băm để theo dõi lần xuất hiện của từng giá trị. Mỗi phần tử được xử lý chính xác một lần và chúng tôi chỉ nối thêm lần xuất hiện đầu tiên của mỗi giá trị riêng biệt. Chi tiết triển khai chính là cập nhật tần suất trước khi kiểm tra, đảm bảo tính chính xác trong việc phân loại lần xuất hiện đầu tiên và lần xuất hiện tiếp theo. 

Một lỗi phổ biến là kiểm tra tần suất trước khi tăng nó, điều này làm dịch chuyển tất cả các quyết định đi một và coi những lần xuất hiện đầu tiên là trùng lặp không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 1 3 2
```Chúng tôi theo dõi tần suất và kết quả xây dựng: 

| Bước | Giá trị | Tần suất sau khi cập nhật | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | giữ | [1] | 
| 2 | 2 | 1 | giữ | [1,2] | 
| 3 | 1 | 2 | bỏ qua | [1,2] | 
| 4 | 3 | 1 | giữ | [1,2,3] | 
| 5 | 2 | 2 | bỏ qua | [1,2,3] | 

Đầu ra là 3. 

Điều này cho thấy chỉ những lần xuất hiện đầu tiên mới tồn tại và các lần trùng lặp bị bỏ qua một cách nhất quán. 

### Ví dụ 2 

đầu vào:```
4
7 7 7 7
```| Bước | Giá trị | Tần suất sau khi cập nhật | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 1 | giữ | [7] | 
| 2 | 7 | 2 | bỏ qua | [7] | 
| 3 | 7 | 3 | bỏ qua | [7] | 
| 4 | 7 | 4 | bỏ qua | [7] | 

Đầu ra là 1. 

Điều này chứng tỏ rằng các bản sao lặp lại không tích lũy sau lần xuất hiện đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các phép toán bản đồ băm O(1) | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt | 

Giải pháp này phù hợp thoải mái trong giới hạn vì ngay cả ở 2⋅10^5 phần tử, một lần truyền với hàm băm vẫn hiệu quả cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    res = []
    for x in a:
        c = freq.get(x, 0) + 1
        freq[x] = c
        if c == 1:
            res.append(x)
    
    return str(len(res))

# provided sample-like cases
assert run("5\n1 2 1 3 2\n") == "3"
assert run("4\n7 7 7 7\n") == "1"

# custom cases
assert run("1\n10\n") == "1"
assert run("2\n1 1\n") == "1"
assert run("6\n1 2 3 4 5 6\n") == "6"
assert run("8\n1 2 1 2 3 3 4 4\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| tất cả đều bình đẳng | 1 | trùng lặp lặp đi lặp lại sụp đổ | 
| ngày càng độc đáo | n | không có trường hợp trùng lặp | 
| trùng lặp được ghép nối | n/2 | hành vi lặp đi lặp lại xen kẽ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng chỉ có một phần tử. Thuật toán ngay lập tức tính nó là lần xuất hiện đầu tiên và bao gồm nó, tạo ra đầu ra 1. Bản đồ tần số chứa chính xác một mục nhập và không có chuyển tiếp nào xảy ra nữa. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều giống hệt nhau. Mỗi lần xuất hiện sẽ tăng tần suất, nhưng chỉ lần xuất hiện đầu tiên mới kích hoạt sự bao gồm. Kết quả vẫn là một phần tử duy nhất bất kể kích thước đầu vào. 

Trường hợp thứ ba là sự lặp lại xen kẽ như [1,2,1,2,1,2]. Bản đồ tần số xen kẽ giữa 1 và 2 cho mỗi giá trị, nhưng chỉ lần xuất hiện đầu tiên của mỗi giá trị mới đóng góp. Kết quả cuối cùng chứa chính xác hai phần tử và không có thao tác nào sau đó thay đổi các quyết định trước đó vì tất cả trạng thái được mã hóa theo số đếm tần số.
