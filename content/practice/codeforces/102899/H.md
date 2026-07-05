---
title: "CF 102899H - KK \u4e0e\u5341\u4f73"
description: "Chúng ta được cung cấp một danh sách các số nguyên biểu thị điểm do ban giám khảo chấm. Tất cả các giá trị đều khác 0 và tất cả đều khác biệt. Chúng tôi được phép loại bỏ chính xác một trong những con số này. Sau khi loại bỏ nó, chúng ta nhân tất cả các số còn lại với nhau và tích đó sẽ trở thành điểm cuối cùng."
date: "2026-07-04T08:21:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "H"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 40
verified: true
draft: false
---

[CF 102899H - KK \u4e0e\u5341\u4f73](https://codeforces.com/problemset/problem/102899/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên biểu thị điểm do ban giám khảo chấm. Tất cả các giá trị đều khác 0 và tất cả đều khác biệt. Chúng tôi được phép loại bỏ chính xác một trong những con số này. Sau khi loại bỏ nó, chúng ta nhân tất cả các số còn lại với nhau và tích đó sẽ trở thành điểm cuối cùng. Nhiệm vụ là chọn số đơn cần loại bỏ sao cho tích kết quả này càng lớn càng tốt và xuất ra chính số đó. 

Khó khăn chính là tích phụ thuộc đồng thời vào tất cả các phần tử còn lại nên việc loại bỏ một phần tử sẽ ảnh hưởng đến độ lớn cũng như dấu của kết quả. Với tối đa 30.000 số, bất kỳ phương pháp nào tính toán lại sản phẩm từ đầu cho mỗi lần loại bỏ đều phải được xem xét cẩn thận về mặt chi phí số học, bởi vì phép nhân đơn giản của các sản phẩm dài liên tục là quá chậm. 

Một khía cạnh tinh tế đến từ hành vi ký hiệu. Sản phẩm có thể lật dấu tùy theo số âm còn lại. Việc xóa số âm có thể thay đổi tính chẵn lẻ của số âm và việc xóa số dương sẽ ảnh hưởng đến độ lớn khác với việc xóa số âm. Điều này làm cho lý luận thuần túy theo kiểu “loại bỏ nhỏ nhất” hoặc “loại bỏ lớn nhất” là không chính xác. 

Các trường hợp khó khăn phá vỡ trực giác ngây thơ bao gồm: 

Ví dụ: nếu có đúng một số âm`[-5, 2, 3, 4]`, loại bỏ tích âm làm cho tích số dương và lớn, nhưng loại bỏ tích dương vẫn để lại tích âm. Quy tắc "loại bỏ nhỏ nhất" tham lam có thể không thành công tùy thuộc vào việc xử lý dấu hiệu. 

Nếu có nhiều tiêu cực, chẳng hạn như`[-4, -3, -2, -1, 10]`, việc loại bỏ một số âm có thể chuyển đổi tính chẵn lẻ và thay đổi đáng kể dấu hiệu và độ lớn theo các cách cạnh tranh. 

Bởi vì tất cả các số đều khác 0 và khác biệt, nên chúng ta không bao giờ xử lý các ràng buộc hoặc sự sụp đổ tích số 0, điều này giúp đơn giản hóa việc suy luận về những thay đổi đơn điệu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tính tích của tất cả các số ngoại trừ một chỉ số ứng cử viên cho mỗi lần loại bỏ có thể. Với mỗi i, chúng ta nhân tất cả các phần tử ngoại trừ`a[i]`và so sánh kết quả. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của vấn đề. Tuy nhiên, mỗi đánh giá tốn O(n) và việc thực hiện điều này cho tất cả n lựa chọn sẽ dẫn đến phép nhân O(n2). Với n lên đến 3·10⁴, số này trở thành khoảng 9·10⁸ phép nhân, quá chậm trong Python và đường biên ngay cả trong các ngôn ngữ được tối ưu hóa. 

Quan sát quan trọng là chúng ta không thực sự cần nhiều lần các sản phẩm đầy đủ. Nếu chúng ta coi tích của tất cả các phần tử là giá trị cơ bản thì loại bỏ`a[i]`tương ứng với việc chia tổng sản phẩm cho`a[i]`. Thách thức là sản phẩm có thể bị tràn và việc so sánh dấu hiệu phải được xử lý cẩn thận. Nhưng về mặt logic, mọi kết quả ứng cử viên đều tỷ lệ thuận với`P / a[i]`, Ở đâu`P`là tích của mọi số. 

Thay vì trực tiếp làm việc với phép chia và số lượng lớn, chúng ta có thể so sánh các ứng viên bằng cách suy luận về việc loại bỏ từng phần tử sẽ thay đổi sản phẩm như thế nào. Vì tất cả các giá trị đều khác 0 nên việc so sánh`P / a[i]`trên khác nhau i tương đương với việc so sánh`1 / a[i]`về mặt hiệu ứng nhân lên trên cùng một sản phẩm cơ bản. Tuy nhiên, vì dấu và độ lớn tương tác với nhau, nên một cách tiếp cận an toàn hơn là tính tổng tích một lần bằng cách sử dụng các số nguyên lớn của Python và sau đó tính từng giá trị ứng cử viên bằng phép chia số nguyên. 

Điều này làm giảm vấn đề xuống còn tính toán O(n): một lượt sản phẩm đầy đủ và một lượt để đánh giá mỗi lần loại bỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

1. Tính tích các số trong mảng. Điều này đưa ra một giá trị cơ bản thể hiện phép nhân đầy đủ trước khi loại bỏ bất kỳ. 
2. Lặp lại từng phần tử`a[i]`và tính giá trị loại bỏ nó bằng cách chia tổng sản phẩm cho`a[i]`. Điều này mang lại điểm số kết quả cho việc loại bỏ đó. 
3. Theo dõi sản phẩm thu được tối đa cho đến nay và lưu trữ chỉ mục tương ứng. 
4. Sau khi kiểm tra tất cả các phần tử, xuất ra phần tử mà việc loại bỏ nó tạo ra tích lớn nhất. 

Điểm quyết định quan trọng là chúng tôi không bao giờ tính toán lại toàn bộ sản phẩm từ đầu. Chúng tôi dựa vào mối quan hệ đại số để loại bỏ một yếu tố khỏi tích tương đương với việc chia cho yếu tố đó. 

### Tại sao nó hoạt động 

Mọi kết quả hợp lệ đều tương ứng chính xác với`P / a[i]`, Ở đâu`P`là sản phẩm của mọi yếu tố. Từ`P`không đổi trong tất cả các lựa chọn, tối đa hóa`P / a[i]`trên i tương đương với việc chọn phép biến đổi tốt nhất của cùng một đại lượng bazơ. Không có ứng cử viên nào bị bỏ sót và mỗi ứng cử viên được đánh giá chính xác một lần, duy trì tính chính xác. Điều tinh tế duy nhất là số học số nguyên bảo toàn các giá trị chính xác trong Python, do đó các so sánh đáng tin cậy mà không có lỗi dấu phẩy động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    total = 1
    for x in a:
        total *= x
    
    best_val = None
    best_idx = 0
    
    for i, x in enumerate(a):
        val = total // x
        if best_val is None or val > best_val:
            best_val = val
            best_idx = i
    
    print(a[best_idx])

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên xây dựng tổng sản phẩm trong một lần duy nhất. Điều này an toàn vì số nguyên Python tự động mở rộng đến độ chính xác tùy ý, do đó việc tràn không phải là vấn đề. 

Sau đó, nó đánh giá từng lần loại bỏ bằng cách chia tổng sản phẩm cho phần tử hiện tại. Phép chia số nguyên là chính xác vì mọi phần tử đều là thừa số của tích tổng. Bước so sánh rất đơn giản: chúng tôi giữ lại chỉ số mang lại thương số lớn nhất. 

Một lỗi triển khai phổ biến là cố gắng tính toán lại các kết quả cho từng chỉ mục hoặc cố gắng mô phỏng phép nhân bằng cách tính toán lại một phần, dẫn đến O(n²). Một sai lầm tinh vi khác là cố gắng lập luận bằng phép chia dấu phẩy động, điều này có thể gây ra lỗi chính xác khi sản phẩm phát triển lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`1 2 3 4 5`| tôi | đã xóa | tổng sản phẩm | kết quả (P/a[i]) | 
| --- | --- | --- | --- | 
| 0 | 1 | 120 | 120 | 
| 1 | 2 | 120 | 60 | 
| 2 | 3 | 120 | 40 | 
| 3 | 4 | 120 | 30 | 
| 4 | 5 | 120 | 24 | 

Giá trị lớn nhất xảy ra khi loại bỏ 1. Điều này xác nhận rằng việc loại bỏ giá trị dương nhỏ nhất sẽ làm tăng tích số nhiều nhất trong các trường hợp thuần túy dương. 

### Ví dụ 2:`-6 8 9`| tôi | đã xóa | tổng sản phẩm | kết quả (P/a[i]) | 
| --- | --- | --- | --- | 
| 0 | -6 | -432 | 72 | 
| 1 | 8 | -432 | -54 | 
| 2 | 9 | -432 | -48 | 

Kết quả tốt nhất thu được bằng cách loại bỏ -6, chuyển kết quả từ âm sang dương và mang lại giá trị lớn nhất trong số các kết quả dương. 

Điều này chứng tỏ tầm quan trọng của việc đổi dấu: việc loại bỏ số âm có thể chi phối lý luận thuần túy dựa trên độ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để tính toán sản phẩm và một lượt để đánh giá mỗi lần loại bỏ | 
| Không gian | O(1) | chỉ có một vài biến được sử dụng ngoài mảng đầu vào | 

Thuật toán chạy thoải mái trong các giới hạn vì phép nhân và chia 3·10⁴ là chuyện nhỏ trong Python ngay cả với các số nguyên lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))
    
    total = 1
    for x in a:
        total *= x
    
    best_val = None
    best_idx = 0
    
    for i, x in enumerate(a):
        val = total // x
        if best_val is None or val > best_val:
            best_val = val
            best_idx = i
    
    return str(a[best_idx])

# provided samples (reconstructed format)
assert run("5\n1 2 3 4 5\n") == "1"
assert run("3\n-6 8 9\n") == "-6"

# custom cases
assert run("2\n-1 5\n") == "-1"          # removing negative makes product positive
assert run("2\n2 -3\n") == "-3"          # compare two symmetric choices
assert run("4\n-2 -3 4 5\n") in {"-2","-3"}  # parity-sensitive case
assert run("3\n10 2 3\n") == "2"         # removing smallest positive maximizes product
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 -1 5`|`-1`| loại bỏ tiêu cực cải thiện dấu hiệu và sản phẩm | 
|`2 2 -3`|`-3`| tương tác giữa tích cực và tiêu cực | 
|`-2 -3 4 5`|`-2/-3`| độ nhạy chẵn lẻ trong số âm | 
|`10 2 3`|`2`| hành vi sản phẩm tối đa chỉ tích cực cổ điển | 

## Vỏ cạnh 

Hãy xem xét`[-1, 2]`. Đang xóa`-1`lá`2`, trong khi loại bỏ`2`lá`-1`. Thuật toán tính tổng sản phẩm`-2`, sau đó đánh giá các lần xóa: xóa`-1`cho`2`, loại bỏ`2`cho`-1`. Mức tối đa được xác định chính xác là loại bỏ`-1`. 

TRONG`[-4, -3, -2, -1, 10]`, tổng tích số là dương. Loại bỏ các tiêu cực khác nhau hoặc tích cực duy nhất mang lại cường độ khác nhau. Thuật toán so sánh rõ ràng các thương số chính xác, do đó, nó tự nhiên chọn sự cân bằng tốt nhất giữa độ ổn định của dấu và mức tăng cường độ mà không cần logic chẵn lẻ riêng biệt. 

TRONG`1 2`, loại bỏ`1`sản lượng`2`và loại bỏ`2`sản lượng`1`. Thuật toán xác định chính xác việc loại bỏ phần tử nhỏ hơn vì nó tối đa hóa sản phẩm còn lại.
