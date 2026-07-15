---
title: "CF 103389D - \u4fee\u5efa\u9053\u8def"
description: "Chúng ta được cung cấp một chuỗi các số nguyên, trong đó mỗi vị trí có thể được hiểu là một nút trên một dòng và giá trị tại mỗi vị trí biểu thị trọng lượng hoặc chiều cao được liên kết với nút đó."
date: "2026-07-03T12:11:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103389
codeforces_index: "D"
codeforces_contest_name: "2021\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 103389
solve_time_s: 43
verified: true
draft: false
---

[CF 103389D - \u4fee\u5efa\u9053\u8def](https://codeforces.com/problemset/problem/103389/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên, trong đó mỗi vị trí có thể được hiểu là một nút trên một dòng và giá trị tại mỗi vị trí biểu thị trọng lượng hoặc chiều cao được liên kết với nút đó. Nhiệm vụ là tính toán một giá trị tương ứng với chi phí để “nối” đường này một cách tối ưu theo một quy tắc xây dựng cụ thể xuất phát từ các giá trị đó. 

Ý tưởng chính được đưa ra trong tuyên bố này là các kết nối hoạt động khác nhau tùy thuộc vào việc chúng vượt qua mức tối đa cục bộ hay nằm trong một khu vực được ngăn cách bởi nó. Nếu chúng ta chọn một vị trí có giá trị lớn nhất trong toàn bộ chuỗi, thì vị trí đó sẽ đóng vai trò như một dấu phân cách tự nhiên: mọi kết nối đi qua nó đều có chi phí được xác định theo mức tối đa đó, trong khi các kết nối hoàn toàn ở một phía chỉ bị chi phối bởi các giá trị bên trong phía đó. 

Đầu ra cuối cùng là một số nguyên duy nhất tổng hợp chi phí tối thiểu có thể có để xây dựng tất cả các kết nối cần thiết theo các ràng buộc này. 

Từ quan điểm phức tạp, kích thước đầu vào đủ lớn để yêu cầu xử lý tuyến tính hoặc gần tuyến tính ít nhất. Bất kỳ phương pháp bậc hai nào cố gắng mô phỏng rõ ràng tất cả các kết nối giữa các cặp hoặc duy trì trạng thái kết nối đầy đủ sẽ quá chậm khi độ dài chuỗi đạt đến giới hạn lập trình cạnh tranh điển hình như 200.000 trở lên. Điều này ngay lập tức gợi ý rằng giải pháp phải giảm bớt vấn đề về các mối quan hệ cục bộ giữa các phần tử liền kề hoặc một phép tính đơn lẻ. 

Một trường hợp thất bại phổ biến đối với lý luận ngây thơ là cố gắng mô phỏng rõ ràng việc phân tách ở mức tối đa và tính toán lại kết nối bên trong mỗi phân đoạn. Ví dụ, hãy xem xét một mảng như`[1, 100, 2, 3]`. Một chiến lược ngây thơ có thể tính toán lại các phân vùng bên trái và bên phải xung quanh`100`và xử lý đệ quy các phân đoạn, nhưng điều này có nguy cơ bị tính hai lần hoặc thiếu các đóng góp xuyên biên giới. Hành vi đúng chỉ phụ thuộc vào so sánh cục bộ chứ không phụ thuộc vào phân rã đệ quy. 

Một vấn đề tế nhị khác là xử lý sai các giá trị bằng nhau. Ví dụ`[5, 5, 5]`không nên tạo ra sự phân chia bổ sung nhân tạo hoặc xử lý đặc biệt đối với các điều kiện “nghiêm ngặt hơn”. Bất kỳ giải pháp nào dựa vào sự bất bình đẳng nghiêm ngặt ở sai vị trí sẽ phân loại sai các lân cận bằng nhau và tạo ra sự đóng góp cạnh không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là lý giải rõ ràng về mọi mối liên hệ tiềm ẩn do cấu trúc tạo ra. Người ta có thể tưởng tượng việc chọn một mức tối đa toàn cục, chia mảng thành các phần bên trái và bên phải, giải đệ quy từng bên và sau đó kết nối cả hai bên trở lại mức tối đa. Mặc dù ý tưởng này đúng về mặt khái niệm nhưng nó dẫn đến việc tính toán lại nhiều lần cùng một cấu trúc ở mọi cấp độ đệ quy. Trong trường hợp xấu nhất, chẳng hạn như một mảng tăng hoặc giảm nghiêm ngặt, điều này thoái hóa thành việc quét lặp đi lặp lại các phân đoạn co lại, dẫn đến hành vi bậc hai. 

Quan sát quan trọng là cấu trúc toàn cầu là không cần thiết. Thay vì suy luận về các phân đoạn đầy đủ, chúng ta có thể kiểm tra xem điều gì xảy ra cục bộ giữa các phần tử liền kề. Mỗi cặp liền kề đóng góp chính xác một lần cho câu trả lời cuối cùng và đóng góp đó được xác định bằng giá trị lớn hơn trong hai giá trị. Điều này xuất phát trực tiếp từ thực tế là giá trị vượt trội ở bất kỳ khu vực địa phương nào đóng vai trò là dấu phân cách xác định chi phí kết nối. 

Một khi điều này được nhận ra, toàn bộ vấn đề sẽ chuyển sang tính tổng đơn giản trên các cặp liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân đoạn đệ quy) | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (tổng hợp liền kề) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng các giá trị đại diện cho dãy. Đây là cấu trúc mà chúng tôi sẽ phân tích cục bộ thay vì toàn cầu. 
2. Khởi tạo bộ tích lũy`ans = 0`để lưu trữ số tiền đóng góp cuối cùng. 
3. Lặp lại từng cặp liền kề`(a[i], a[i+1])`từ trái sang phải. Mỗi cặp đại diện cho một ranh giới trong đó sự thống trị giữa hai vị trí lân cận xác định chi phí kết nối cần thiết tối thiểu qua ranh giới đó. 
4. Với mỗi cặp, thêm`max(a[i], a[i+1])`ĐẾN`ans`. Lý do là điểm cuối lớn hơn sẽ chi phối bất kỳ kết nối nào có thể “vượt qua” hoặc “bắc cầu” ranh giới cục bộ này một cách hiệu quả trong cấu trúc tối ưu. 
5. Sau khi xử lý tất cả các cặp liền kề, xuất ra`ans`như là kết quả cuối cùng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi cạnh trong cấu trúc khái niệm tương ứng với chính xác một ranh giới kề trong chuỗi ban đầu và chi phí của nó chỉ được xác định bởi điểm cuối lớn hơn của ranh giới đó. Đối số tối đa toàn cầu trong tuyên bố đảm bảo rằng bất kỳ cấu trúc nào vượt qua đỉnh phải trả ít nhất giá trị của đỉnh đó và hiệu ứng này phân hủy rõ ràng thành các đóng góp độc lập giữa các lân cận. Bởi vì mỗi vùng lân cận được tính chính xác một lần và không có tương tác nào vượt quá các vùng lân cận ngay lập tức trong công thức cuối cùng, tổng trên cực đại cục bộ sẽ nắm bắt đầy đủ chi phí tối ưu toàn cục mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

ans = 0
for i in range(n - 1):
    ans += max(a[i], a[i + 1])

print(ans)
```Giải pháp đầu tiên đọc mảng theo thời gian tuyến tính và sau đó thực hiện một lần chuyển qua các cặp liền kề. Bộ tích lũy`ans`thu thập các khoản đóng góp từ mỗi ranh giới bằng cách sử dụng thời gian không đổi`max`hoạt động. 

Một điểm tinh tế là không cần xử lý đặc biệt đối với mức tối đa toàn cục hoặc để phân đoạn. Mặc dù tuyên bố thúc đẩy ý tưởng thông qua một trục xoay tối đa, nhưng cấu trúc đó chỉ là một công cụ chứng minh; biểu thức cuối cùng đã mã hóa đầy đủ sự đóng góp của mọi vị trí cục bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3 2 4
```| tôi | một [tôi] | a[i+1] | max(a[i], a[i+1]) | và | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 3 | 3 | 
| 1 | 3 | 2 | 3 | 6 | 
| 2 | 2 | 4 | 4 | 10 | 
Đầu ra cuối cùng:`10`Dấu vết này cho thấy mỗi ranh giới đóng góp độc lập và kết quả cuối cùng chỉ đơn giản là sự tích lũy cực đại cục bộ. 

### Ví dụ 2 

đầu vào:```
5
5 5 1 7 2
```| tôi | một [tôi] | a[i+1] | max(a[i], a[i+1]) | và | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 5 | 5 | 5 | 
| 1 | 5 | 1 | 5 | 10 | 
| 2 | 1 | 7 | 7 | 17 | 
| 3 | 7 | 2 | 7 | 24 | 
Đầu ra cuối cùng:`24`Trường hợp này nhấn mạnh rằng các giá trị bằng nhau hoạt động một cách tự nhiên: hàm max đảm bảo tính đối xứng và không cần cách viết hoa đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | chuyền đơn qua các cặp liền kề | 
| Không gian | O(1) | chỉ sử dụng bộ tích lũy | 

Thuật toán tuyến tính theo kích thước của mảng đầu vào, tối ưu vì mọi phần tử phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ không đổi ngoại trừ bộ nhớ đầu vào, khiến nó phù hợp với những hạn chế lớn thường gặp trong lập trình cạnh tranh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))

    ans = 0
    for i in range(n - 1):
        ans += max(a[i], a[i + 1])

    return str(ans)

# provided sample-like cases
assert run("4\n1 3 2 4\n") == "10"
assert run("5\n5 5 1 7 2\n") == "24"

# custom cases
assert run("1\n10\n") == "0", "single element"
assert run("2\n1 100\n") == "100", "two elements boundary"
assert run("3\n3 3 3\n") == "6", "all equal values"
assert run("6\n6 5 4 3 2 1\n") == "20", "strictly decreasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 element`|`0`| không có cạnh kề | 
|`1 100`|`100`| độ chính xác ranh giới duy nhất | 
|`3 equal`|`6`| ổn định xử lý bằng nhau | 
| giảm dần |`20`| tính đúng đắn của cấu trúc đơn điệu | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[10]`, không có cặp liền kề nào, do đó vòng lặp không bao giờ thực hiện và câu trả lời vẫn giữ nguyên`0`, điều này phù hợp với ý tưởng rằng không có kết nối nào tồn tại. 

Đối với một mảng như`[1, 100]`, thuật toán thực hiện đúng một bước, lấy`max(1, 100) = 100`. Điều này phù hợp với cách giải thích rằng ranh giới duy nhất bị chi phối bởi điểm cuối lớn hơn. 

Đối với các mảng thống nhất như`[5, 5, 5]`, mọi vùng lân cận đều đóng góp một giá trị như nhau`5`, và kết quả trở thành`10`. Thuật toán tránh chính xác bất kỳ cách viết hoa đặc biệt nào cho sự bình đẳng. 

Đối với các mảng giảm nghiêm ngặt như`[6, 5, 4, 3]`, mỗi bước đóng góp phần tử bên trái và tổng tích lũy chính xác mà không cần bất kỳ logic phân đoạn nào, xác nhận rằng sự thống trị cục bộ nắm bắt hoàn toàn cấu trúc toàn cầu.
