---
title: "CF 105434B - \u5168\u6982\u7387\u516c\u5f0f"
description: "Chúng tôi được cung cấp một số nguồn dữ liệu độc lập, mỗi nguồn đại diện cho một nền tảng có thể đóng góp vào tập dữ liệu hỗn hợp. Mỗi nền tảng đóng góp một phần đã biết trong tổng số dữ liệu và mỗi nền tảng cũng có xác suất bị “nhiễm AI” riêng."
date: "2026-06-23T03:51:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105434
codeforces_index: "B"
codeforces_contest_name: "2024\u5e74\u201c\u6838\u6843\u676f\u201d\u6b66\u6c49\u5730\u533aACM\u840c\u65b0\u8d5b"
rating: 0
weight: 105434
solve_time_s: 56
verified: true
draft: false
---

[CF 105434B - \u5168\u6982\u7387\u516c\u5f0f](https://codeforces.com/problemset/problem/105434/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số nguồn dữ liệu độc lập, mỗi nguồn đại diện cho một nền tảng có thể đóng góp vào tập dữ liệu hỗn hợp. Mỗi nền tảng đóng góp một phần đã biết trong tổng số dữ liệu và mỗi nền tảng cũng có xác suất bị “nhiễm AI” riêng. 

Nói một cách cụ thể hơn, hãy tưởng tượng bạn chọn một phần dữ liệu ngẫu nhiên từ một nhóm dữ liệu kết hợp khổng lồ. Đầu tiên, nền tảng được chọn theo trọng lượng của nó trong hỗn hợp. Sau đó, trong nền tảng đó, dữ liệu sẽ sạch hoặc bị nhiễu do AI tạo ra tùy theo xác suất đã biết. Nhiệm vụ là tính xác suất tổng thể để phần dữ liệu được chọn ngẫu nhiên bị ô nhiễm. 

Đầu vào cung cấp hai phân phối xác suất phù hợp. Phần đầu tiên mô tả khả năng mỗi nền tảng sẽ được chọn. Các xác suất này có tổng bằng 1 nên chúng tạo thành một phân bố hỗn hợp hợp lệ. Phần thứ hai mô tả, đối với mỗi nền tảng, xác suất có điều kiện mà dữ liệu từ nền tảng đó bị ô nhiễm. 

Đầu ra là xác suất biên của ô nhiễm trên toàn bộ hỗn hợp. 

Các ràng buộc rất nhỏ: tối đa 100 nền tảng, do đó, ngay cả giải pháp O(n) hoặc O(n log n) cũng không đáng kể. Điều tinh tế duy nhất là độ ổn định về số, vì câu trả lời là tổng có trọng số dấu phẩy động và dung sai lỗi yêu cầu là tương đối chứ không phải tuyệt đối. Điều này có nghĩa là các giá trị gần bằng 0 cần phải cẩn thận nhưng độ chính xác kép tiêu chuẩn là đủ. 

Một lỗi phổ biến là coi các xác suất là độc lập và nhân tất cả các giá trị với nhau hoặc tính trung bình các xác suất có điều kiện mà không tính trọng số. Ví dụ: nếu một nền tảng đóng góp 99% dữ liệu và nền tảng khác đóng góp 1%, thì chỉ cần tính trung bình tỷ lệ ô nhiễm của chúng sẽ làm sai lệch nghiêm trọng kết quả. Việc cân trọng lượng là cần thiết. 

Sai lầm thứ hai là quên rằng xác suất của nền tảng đã tạo thành một phân phối hoàn chỉnh. Việc thêm các bước chuẩn hóa hoặc chuẩn hóa lại có thể gây ra lỗi dấu phẩy động không cần thiết. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng quá trình hai giai đoạn. Người ta có thể lấy mẫu nhiều lần một nền tảng theo phân bố xác suất của nó, sau đó lấy mẫu xem nó có bị ô nhiễm hay không và ước tính kết quả thông qua Monte Carlo. Mặc dù đúng về mặt khái niệm, nhưng cách tiếp cận này không cần thiết và không chính xác, và ngay cả với số lượng lớn các mô phỏng, nó cũng chỉ gần đúng cho câu trả lời. Phương sai cũng sẽ phụ thuộc vào độ lệch của xác suất và không có gì đảm bảo đáp ứng yêu cầu nghiêm ngặt về lỗi tương đối. 

Quan sát quan trọng là xác suất ô nhiễm là sự áp dụng trực tiếp định luật tổng xác suất. Mỗi nền tảng đóng góp độc lập cho kỳ vọng cuối cùng, được tính theo tần suất được chọn. Thay vì mô phỏng, chúng ta có thể tính toán kỳ vọng chính xác trong một lần bằng cách tính tổng các đóng góp ở dạng “xác suất chọn nền tảng i nhân với xác suất ô nhiễm cho nền tảng i”. 

Điều này thu gọn toàn bộ vấn đề thành một tập hợp tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Monte Carlo | O(k) cho mẫu k lớn | O(1) | Quá chậm/không chính xác | 
| Tổng có trọng số (Tối ưu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số sân ga n. Mỗi nền tảng đại diện cho một nguồn dữ liệu loại trừ lẫn nhau, do đó, chính xác một nền tảng sẽ được chọn trong mô hình xác suất. 
2. Đọc mảng f, trong đó f[i] là xác suất để một mục dữ liệu được chọn ngẫu nhiên có nguồn gốc từ nền tảng i. Các giá trị này có tổng bằng 1 nên chúng tạo thành một phân bố hỗn hợp hợp lệ. 
3. Đọc mảng a, trong đó a[i] là xác suất có điều kiện để một mục dữ liệu từ nền tảng i bị nhiễm AI. 
4. Khởi tạo bộ tích lũy ans = 0. Điều này sẽ lưu trữ tổng khả năng lây nhiễm trên tất cả các nền tảng. 
5. Với mỗi nền tảng i từ 1 đến n, thêm f[i] * a[i] vào ans. Thuật ngữ này thể hiện sự đóng góp của nền tảng i vào tổng xác suất: đầu tiên chọn nền tảng, sau đó quan sát sự ô nhiễm bên trong nó. 
6. Kết quả đầu ra dưới dạng số dấu phẩy động. 

Phép nhân bên trong tổng là bước lập mô hình quan trọng. Mỗi nền tảng đóng góp tỷ lệ thuận với tần suất được chọn, do đó tỷ lệ ô nhiễm của nó phải được điều chỉnh theo xác suất lựa chọn trước khi tổng hợp. 

### Tại sao nó hoạt động 

Gọi A là sự kiện một mục dữ liệu được chọn ngẫu nhiên bị ô nhiễm và Fi là sự kiện mục dữ liệu đó đến từ nền tảng i. Các sự kiện Fi tạo thành một phân vùng của không gian mẫu. Theo định luật tổng xác suất, P(A) bằng tổng của tất cả i của P(Fi) nhân với P(A | Fi). Thuật toán tính toán chính xác sự phân tách này mà không có giả định gần đúng hoặc ẩn. Vì tất cả Fi đều rời rạc và đầy đủ nên không có khối lượng xác suất nào được tính hai lần hoặc bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    f = list(map(float, input().split()))
    a = list(map(float, input().split()))

    ans = 0.0
    for i in range(n):
        ans += f[i] * a[i]

    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp là thực hiện trực tiếp tổng trọng số. Chi tiết duy nhất đáng chú ý là sử dụng số học dấu phẩy động xuyên suốt. Vì n nhỏ nên không cần tối ưu hóa ngoài một vòng lặp đơn giản. 

Phép nhân được thực hiện trước khi tích lũy, đây là thứ tự ổn định về số lượng cho kích thước đầu vào này. Không cần chuẩn hóa hoặc chia tỷ lệ vì đầu vào đã xác định phân bố xác suất hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu trong đó hai nền tảng đóng góp 0,4 và 0,6 dữ liệu và xác suất ô nhiễm của chúng lần lượt là 0,6 và 0,3. 

| tôi | f[i] | một [tôi] | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 0,4 | 0,6 | 0,24 | 
| 2 | 0,6 | 0,3 | 0,18 | 

Tổng trở thành 0,42. 

Dấu vết này cho thấy nền tảng thứ hai ảnh hưởng lớn đến kết quả như thế nào mặc dù tỷ lệ ô nhiễm thấp hơn, hoàn toàn vì nó chi phối việc phân phối dữ liệu. 

Bây giờ hãy xem xét một trường hợp sai lệch: một nền tảng chiếm ưu thế gần như hoàn toàn. 

đầu vào: 

f = [0,99, 0,01] 

a = [0,0, 1,0] 

| tôi | f[i] | một [tôi] | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 0,99 | 0,0 | 0,0 | 
| 2 | 0,01 | 1.0 | 0,01 | 

Kết quả là 0,01, cho thấy một nguồn hiếm nhưng bị ô nhiễm nặng chỉ đóng góp tỷ lệ thuận với tần số của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua n nền tảng tính tổng có trọng số | 
| Không gian | O(1) | Chỉ lưu trữ bổ sung liên tục ngoài mảng đầu vào | 

Các ràng buộc cho phép tối đa 100 nền tảng, do đó quá trình quét tuyến tính diễn ra tức thời một cách hiệu quả. Các phép toán dấu phẩy động chiếm ưu thế, nhưng ngay cả những phép toán này cũng không đáng kể ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    f = list(map(float, input().split()))
    a = list(map(float, input().split()))

    ans = 0.0
    for i in range(n):
        ans += f[i] * a[i]

    return str(ans)

# provided sample
assert abs(float(run("2\n0.4 0.6\n0.6 0.3\n")) - 0.42) < 1e-9

# all clean
assert abs(float(run("3\n0.2 0.3 0.5\n0.0 0.0 0.0\n")) - 0.0) < 1e-9

# all contaminated
assert abs(float(run("3\n0.2 0.3 0.5\n1.0 1.0 1.0\n")) - 1.0) < 1e-9

# single platform
assert abs(float(run("1\n1.0\n0.37\n")) - 0.37) < 1e-9

# skewed distribution
assert abs(float(run("2\n0.999 0.001\n0.2 0.9\n")) - (0.999*0.2 + 0.001*0.9)) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 0,42 | tính đúng đắn cơ bản | 
| tất cả số không | 0 | ô nhiễm thoái hóa | 
| tất cả những cái | 1 | ranh giới ô nhiễm đầy đủ | 
| nền tảng duy nhất | 0,37 | cấu trúc tối thiểu | 
| lệch | tổng có trọng số | độ chính xác dưới sự mất cân bằng | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một nền tảng có xác suất 1 và tất cả các nền tảng khác bằng 0. Trong tình huống đó, câu trả lời phải khớp chính xác với xác suất lây nhiễm của nền tảng đó. Thuật toán xử lý việc này một cách tự nhiên vì tất cả các số hạng có trọng số bằng 0 đều biến mất, chỉ để lại một đóng góp. 

Một trường hợp khác là khi xác suất ô nhiễm đều bằng không. Việc tính toán trở thành tổng của các số 0 bất kể trọng lượng của nền tảng, tạo ra số 0 như mong đợi. 

Trường hợp thứ ba là khi tất cả các xác suất ô nhiễm là một. Kết quả thu gọn về tổng của f[i], được đảm bảo bằng 1 vì phân phối đầu vào được chuẩn hóa. Do đó, thuật toán tạo ra chính xác 1 mà không cần xử lý đặc biệt. 

Cuối cùng, khi xác suất cực kỳ gần bằng 0 hoặc một, việc làm tròn dấu phẩy động có thể gây ra các lỗi nhỏ, nhưng khả năng chịu lỗi tương đối cần thiết cho phép tích lũy độ chính xác kép tiêu chuẩn được thực hiện một cách an toàn.
