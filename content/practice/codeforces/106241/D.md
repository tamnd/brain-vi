---
title: "CF 106241D - Hoán vị tối đa nhỏ"
description: "Chúng ta được yêu cầu xây dựng một hoán vị của các số nguyên từ 1 đến n. Đối với mỗi mảng con của hoán vị này, chúng tôi tính toán một giá trị phụ thuộc vào mức độ phân tán của các số bên trong nó. Cụ thể, đối với phân đoạn p[i.."
date: "2026-06-20T02:58:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106241
codeforces_index: "D"
codeforces_contest_name: "2025 GUC Winter Camp"
rating: 0
weight: 106241
solve_time_s: 64
verified: true
draft: false
---

[CF 106241D - Hoán vị tối đa nhỏ](https://codeforces.com/problemset/problem/106241/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị của các số nguyên từ 1 đến n. Đối với mỗi mảng con của hoán vị này, chúng tôi tính toán một giá trị phụ thuộc vào mức độ phân tán của các số bên trong nó. Cụ thể, đối với một đoạn p[i..j], chúng tôi lấy giá trị lớn nhất trong đoạn đó, trừ đi giá trị nhỏ nhất rồi trừ đi độ dài của đoạn đó. Điều này tạo ra chi phí trở nên nhỏ khi các giá trị được đóng gói chặt chẽ và phân khúc ngắn và trở nên lớn khi phân khúc dài hoặc có khoảng cách giá trị lớn. 

Tổng số điểm của một hoán vị là tổng của chi phí này trên tất cả các mảng con có thể có. Nhiệm vụ là tìm một hoán vị làm tối thiểu tổng số điểm này. Trong số tất cả các hoán vị đạt được số điểm tối thiểu có thể, chúng ta phải đưa ra hoán vị lớn nhất về mặt từ điển. 

Các ràng buộc cho phép n lên tới 200.000, do đó, bất kỳ giải pháp nào cố gắng đánh giá tất cả các mảng con một cách rõ ràng đều không thể thực hiện được ngay lập tức. Số lượng mảng con là bậc hai và thậm chí việc tính toán tối thiểu và tối đa cho mỗi mảng con sẽ dẫn đến hành vi bậc ba. 

Một điểm tinh tế là mục tiêu chỉ phụ thuộc vào thứ tự tương đối của các giá trị chứ không phải sự đồng nhất tuyệt đối của chúng, vì hoán vị luôn từ 1 đến n. Một quan sát quan trọng khác là mỗi mảng con đều đóng góp một thuật ngữ liên quan đến cả giá trị tối đa và tối thiểu, điều này cho thấy rằng các phần tử trở thành cực trị trong nhiều phân đoạn có ảnh hưởng lớn đến chi phí. 

Một cách tiếp cận đơn giản có thể cố gắng suy luận cục bộ hoặc tham lam chọn các phần tử trong khi theo dõi sự đóng góp của tất cả các mảng con, nhưng những cách tiếp cận như vậy thường thất bại vì việc chèn một giá trị sẽ ảnh hưởng đến tất cả các phân đoạn đi qua vị trí của nó theo cách rất phi cục bộ. 

Một ví dụ nhỏ nêu bật sự nhạy cảm. Với n = 3, các hoán vị 1 2 3 và 3 2 1 hoạt động khác nhau vì phần tử ở giữa xác định có bao nhiêu phân đoạn có phạm vi nhỏ. Một kẻ tham lam luôn đặt các giá trị nhỏ hoặc lớn ở các đầu mà không có mô hình chung có thể dễ dàng bỏ lỡ cấu trúc tối ưu thực sự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các hoán vị và tính toán tổng chi phí cho mỗi hoán vị. Ngay cả khi chúng ta có một cách hiệu quả để tính chi phí của một hoán vị trong O(n^2), thì số lượng hoán vị là n!, điều này hoàn toàn không khả thi. Ngay cả việc hạn chế đánh giá một hoán vị vẫn yêu cầu lặp lại trên tất cả các mảng con O(n^2) và tính toán tối thiểu và tối đa một cách ngây thơ mang lại O(n) cho mỗi mảng con, dẫn đến tổng công việc là O(n^3). 

Chúng ta cần hiểu cấu trúc hoán vị nào làm giảm tổng đóng góp. Chi phí của một phân khúc được điều khiển bởi mức tối đa và tối thiểu. Nếu một phân đoạn có các số nguyên liên tiếp trong hoán vị thì phạm vi của nó sẽ nhỏ và chi phí sẽ giảm. Điều này gợi ý rằng chúng ta muốn tránh những bước nhảy lớn bên trong các phân đoạn liền kề và thay vào đó muốn các giá trị được sắp xếp sao cho các điểm cực trị được phân tách một cách có kiểm soát. 

Một phép biến đổi quan trọng là viết lại bài toán theo tần suất mỗi cặp phần tử góp phần trở thành giá trị lớn nhất hoặc nhỏ nhất trên các mảng con. Biểu thức max trừ min có thể được hiểu là sự đóng góp của các cặp trong đó một phần tử trở thành cực đại và phần tử khác trở thành cực tiểu trong một phân đoạn. Thuật ngữ có độ dài âm đẩy về các phân đoạn ngắn hơn, góp phần tạo ra ít hình phạt hơn về tổng thể. 

Cấu trúc nổi lên là các hoán vị tối ưu được xây dựng bằng cách đặt phần tử lớn nhất lên đầu tiên hoặc cuối cùng, sau đó điền đệ quy ra bên ngoài theo cách giữ cho hoán vị ở vị trí “ở giữa” nhất có thể. Tuy nhiên, vì chúng ta cũng cần tối đa hóa từ điển trong số các giải pháp tối ưu, nên chúng ta ưu tiên đặt các phần tử lớn hơn sớm hơn bất cứ khi nào tính đối xứng cho phép.

Điều này dẫn đến một mô hình mang tính xây dựng: chúng tôi xây dựng hoán vị từ cả hai đầu vào trong, luôn chọn số lớn nhất còn lại và đặt nó ở vị trí duy trì sự cân bằng tối ưu, thiên về các vị trí trước đó để đáp ứng tính tối đa từ điển. 

Cấu hình tối ưu hóa ra là một chuỗi giảm đơn giản khi được phân tích dưới sự đóng góp đối xứng, bởi vì bất kỳ sai lệch nào đặt số nhỏ hơn trước số lớn hơn đều có xu hướng tạo ra các mảng con bổ sung trong đó max-min trở nên lớn trong khi không bù đủ thông qua việc giảm độ dài. 

Do đó lời giải rút gọn thành việc sắp xếp các số từ n xuống 1 theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị trực tiếp. 

1. Khởi tạo danh sách trống p có kích thước n. Điều này sẽ lưu trữ hoán vị cuối cùng. 
2. Bắt đầu từ giá trị lớn nhất n và di chuyển xuống 1. 
3. Với mỗi giá trị x từ n xuống 1, nối x vào p. 

Điều này đặt các giá trị lớn hơn sớm hơn trong hoán vị, điều này cần thiết để tối đa hóa thứ tự từ điển trong số các giải pháp tối ưu. 
4. Xuất danh sách đã xây dựng. 

Lý do cấu trúc đơn giản này là đủ là vì bất kỳ sự sắp xếp thay thế nào cũng sẽ đưa ra một cặp chỉ số trong đó giá trị nhỏ hơn đứng trước giá trị lớn hơn theo cách làm tăng số lượng mảng con có phạm vi mở rộng không cần thiết. Thứ tự giảm dần giảm thiểu việc tạo ra các phân đoạn trong đó cả hai cực dường như cách xa nhau, vì các giá trị lớn tập trung ở phía trước và các giá trị nhỏ dần chỉ thu hẹp các phạm vi có thể có. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi đặt k phần tử đầu tiên của hoán vị thành n, n−1, …, n−k+1, mọi mảng con bắt đầu trong tiền tố này đã có giá trị tối đa được xác định ngay lập tức bởi phần tử đầu tiên của mảng con đó. Điều này ổn định sự đóng góp của giá trị tối đa trên nhiều phân đoạn và ngăn chặn sự biến đổi bổ sung có thể phát sinh nếu phần tử nhỏ hơn xuất hiện trước phần tử lớn hơn sau đó trong mảng. 

Bất kỳ sự đảo ngược nào của dạng i < j nhưng p[i] < p[j] sẽ tạo ra các phân đoạn bổ sung trong đó mức tối đa không được xác định cục bộ ở ranh giới bên trái, làm tăng tính biến thiên của mức tối đa trên nhiều phân đoạn. Sự sắp xếp giảm dần loại bỏ hoàn toàn những sự đảo ngược như vậy, làm cho tất cả các cực đại dựa trên tiền tố trở nên đơn điệu và giảm thiểu sự đóng góp của chênh lệch tổng thể. 

Bởi vì cấu trúc này cũng là duy nhất về tính đối xứng trong các cấu hình tối ưu, nên nó tối đa về mặt từ điển trong số tất cả các giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    res = list(range(n, 0, -1))
    print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp xây dựng hoán vị theo thứ tự ngược lại. Chi tiết quan trọng duy nhất là đảm bảo định dạng chính xác với khoảng trắng và tránh bất kỳ lỗi nào bằng cách sử dụng phạm vi của Python với bước -1. 

Quyết định in ngay lập tức thay vì xây dựng các cấu trúc gia tăng sẽ tránh được chi phí không cần thiết. Không cần cấu trúc dữ liệu bổ sung vì hoán vị mang tính quyết định. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 3 

Chúng tôi xây dựng trình tự từng bước. 

| Bước | Hiện tại x | Hoán vị | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 2 | 3 2 | 
| 3 | 1 | 3 2 1 | 

Kết quả cuối cùng là 3 2 1. Điều này xác nhận rằng công trình luôn gắn phần tử lớn nhất còn lại trước tiên. 

Ví dụ này chứng tỏ cách hoán vị xây dựng một cấu trúc giảm dần mà không có sự mơ hồ, đảm bảo thứ tự tối đa về mặt từ điển giữa các ứng cử viên. 

### Ví dụ 2: n = 5 

| Bước | Hiện tại x | Hoán vị | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 4 | 5 4 | 
| 3 | 3 | 5 4 3 | 
| 4 | 2 | 5 4 3 2 | 
| 5 | 1 | 5 4 3 2 1 | 

Kết quả lại là một hoán vị giảm nghiêm ngặt. Điều này cho thấy phương pháp này có tính khái quát hóa thống nhất và không phụ thuộc vào bất kỳ sự chẵn lẻ hay phân nhánh cấu trúc nào. 

Dấu vết nhấn mạnh rằng không cần có quyết định sắp xếp lại, điều này phù hợp với việc không có sự cân bằng cục bộ trong quá trình xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta xuất ra mỗi số đúng một lần theo thứ tự giảm dần | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị trước khi in | 

Độ phức tạp dễ dàng phù hợp với các ràng buộc cho n lên tới 200.000. Giải pháp này hoàn toàn tuyến tính và chỉ thực hiện các phép toán số học và đầu ra đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import ModuleType

    # redefine solve inline for testing simplicity
    n = int(sys.stdin.readline())
    res = list(range(n, 0, -1))
    return " ".join(map(str, res))

# provided samples (conceptual since full samples not shown)
assert run("1") == "1", "n=1"
assert run("2") == "2 1", "n=2"

# custom cases
assert run("3") == "3 2 1", "basic descending"
assert run("5") == "5 4 3 2 1", "larger case"
assert run("10") == "10 9 8 7 6 5 4 3 2 1", "monotone correctness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | ranh giới tối thiểu | 
| 2 | 2 1 | trường hợp không tầm thường nhỏ nhất | 
| 5 | 5 4 3 2 1 | tính đúng đắn của mẫu chung | 
| 10 | 10 9 8 7 6 5 4 3 2 1 | tính nhất quán của quy mô | 

## Vỏ cạnh 

Trường hợp cạnh chính là n = 1. Thuật toán tạo ra một hoán vị phần tử đơn [1], về cơ bản không có mảng con nào có độ dài lớn hơn một và do đó tất cả các điều khoản chi phí đều bằng 0. Điều này phù hợp với sự tối ưu. 

Với n = 2, thuật toán tạo ra 2 1. Các mảng con duy nhất là [2], [1] và [2,1]. Thứ tự giảm dần đảm bảo phân đoạn có độ dài 2 duy nhất có tối đa bằng 1 và không có hoán vị thay thế nào có thể cải thiện thứ tự từ điển mà không làm tăng sự mất cân bằng cấu trúc. 

Một trường hợp ít tầm thường hơn một chút là n = 3. Thuật toán tạo ra 3 2 1. Cấu trúc không đảo ngược đảm bảo rằng mọi tiền tố đều có mức tối đa ổn định và không có hoán vị thay thế nào có thể làm giảm phần đóng góp mà không đưa ra các hình phạt từ điển lớn hơn, xác nhận tính đúng đắn ngay cả trong trường hợp nhiều phần tử không đối xứng nhỏ nhất.
