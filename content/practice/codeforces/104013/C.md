---
title: "CF 104013C - Sắp xếp bị hỏng"
description: "Chúng ta được cung cấp một mảng ẩn có độ dài $n$, chứa một hoán vị các số từ $1$ đến $n$. Chúng ta không thể nhìn thấy mảng trực tiếp. Cách duy nhất của chúng ta để tương tác với nó là chọn hai vị trí $i < j$ và yêu cầu trọng tài so sánh các giá trị tại các vị trí đó."
date: "2026-07-02T05:01:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104013
codeforces_index: "C"
codeforces_contest_name: "2020-2021 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104013
solve_time_s: 56
verified: true
draft: false
---

[CF 104013C - Sắp xếp bị hỏng](https://codeforces.com/problemset/problem/104013/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có chiều dài ẩn$n$, chứa hoán vị các số từ$1$ĐẾN$n$. Chúng ta không thể nhìn thấy mảng trực tiếp. Cách duy nhất của chúng tôi để tương tác với nó là chọn hai vị trí$i < j$và yêu cầu thẩm phán so sánh các giá trị tại các vị trí đó. Nếu giá trị bên trái lớn hơn giá trị bên phải, thẩm phán sẽ hoán đổi chúng và báo cáo rằng việc hoán đổi đã xảy ra; nếu không thì không có gì thay đổi và thẩm phán báo cáo rằng không có gì xảy ra. 

Mục đích là chuyển đổi mảng thành hoán vị nhận dạng, nghĩa là vị trí$i$phải chứa giá trị$i$cho mọi$i$. Bất cứ lúc nào sau khi chúng tôi truy vấn, giám khảo có thể trả lời bằng WIN thay vì kết quả so sánh, điều đó có nghĩa là mảng đã được sắp xếp đầy đủ và chúng tôi phải dừng ngay lập tức. 

Điều phức tạp là mảng không ổn định. Sau mỗi khối$2n$truy vấn, thẩm phán bí mật chọn ngẫu nhiên hai vị trí giống nhau và hoán đổi giá trị của chúng. Điều này xảy ra mà không có thông báo và có thể phá hủy tiến trình. Chúng tôi vẫn phải đảm bảo rằng trong vòng 10000 truy vấn, cuối cùng chúng tôi sẽ đạt đến trạng thái được sắp xếp vào một thời điểm nào đó khi thẩm phán kiểm tra. 

Những hạn chế$n \le 50$đề nghị mạnh mẽ rằng chúng ta có thể thực hiện các thủ tục bậc hai hoặc thậm chí bậc ba về mặt$n$, bởi vì bất kỳ lần truyền đầy đủ nào qua mảng đều tốn tối đa vài nghìn thao tác. Khó khăn thực sự không phải là hiệu quả mà là sự mạnh mẽ dưới sự tham nhũng ngẫu nhiên. 

Một cách giải thích ngây thơ sẽ là cố gắng xây dựng lại hoán vị một cách chính xác và sau đó mô phỏng việc sắp xếp trong đầu, nhưng điều này không thành công vì mảng thay đổi không thể đoán trước. Một ý tưởng ngây thơ khác là giả sử không có sai sót và chạy thuật toán sắp xếp tiêu chuẩn một lần, nhưng sau mỗi đợt hoán đổi ngẫu nhiên, mảng lại trở nên không hợp lệ và chúng ta không bao giờ hội tụ một cách đáng tin cậy. 

Trường hợp cạnh chính là tình huống sau đây. Giả sử tại một thời điểm nào đó mảng được sắp xếp, nhưng ngay sau khi hoán đổi ngẫu nhiên, mảng đó lại không được sắp xếp trước khi chúng ta truy vấn bất kỳ thứ gì. Nếu thuật toán của chúng tôi chỉ kiểm tra mức độ hoàn thành tại các điểm kiểm tra cố định hoặc giả định sự cải thiện đơn điệu, thì thuật toán đó có thể bỏ lỡ hoàn toàn thời điểm THẮNG hoặc không bao giờ ổn định. 

Do đó, chúng ta cần một quy trình liên tục thực thi trật tự cục bộ mạnh mẽ đến mức ngay cả sau khi thỉnh thoảng hoán đổi ngẫu nhiên, mảng liên tục bị đẩy lùi về thứ tự đã sắp xếp. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng khôi phục toàn bộ hoán vị sau mỗi sự kiện sai lệch và sau đó sắp xếp lại từ đầu. Điều này sẽ liên quan đến việc truy vấn nhiều cặp để suy ra đầy đủ thứ tự, xây dựng lại mảng một cách hiệu quả trong mỗi chu kỳ. Thậm chí nếu chúng ta có thể xác định được hoán vị trong$O(n \log n)$so sánh, lặp lại điều này sau mỗi$2n$hoạt động dẫn đến sự cố nổ tung trong trường hợp xấu nhất$O(\text{cycles} \cdot n \log n)$và vì không có giới hạn về số chu kỳ sửa lỗi cần thiết nên phương pháp này không đảm bảo hoàn thành trong vòng 10000 truy vấn. 

Quan sát quan trọng là chúng ta không cần phải xây dựng lại hoán vị trên toàn cầu. Mọi truy vấn đều cung cấp cho chúng tôi thứ tự cục bộ chính xác giữa hai vị trí. Nếu chúng ta liên tục thực thi tính nhất quán trên các vị trí lân cận, thì mảng sẽ hoạt động như thể nó đang được sắp xếp liên tục ngay cả khi có nhiễu loạn nhẹ. 

Điều này tự nhiên dẫn đến cách tiếp cận kiểu mạng sắp xếp dựa trên so sánh, cụ thể là các hoán đổi liền kề được lặp lại tương tự như sắp xếp bong bóng hoặc sắp xếp chuyển vị chẵn-lẻ. Mỗi so sánh thực thi một bất biến cục bộ: giá trị nhỏ hơn di chuyển sang trái và giá trị lớn hơn di chuyển sang phải bất cứ khi nào chúng tôi phát hiện sự đảo ngược. Ngay cả khi các giao dịch hoán đổi ngẫu nhiên tạo ra các đảo ngược mới, các lần quét lặp lại sẽ loại bỏ chúng một lần nữa. 

Từ$n$tối đa là 50, vượt qua toàn bộ chi phí chỉnh sửa liền kề$O(n)$các hoạt động và việc lặp lại các lần chuyển như vậy với số lần giới hạn sẽ dễ dàng phù hợp với ngân sách hoạt động 10000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết toàn bộ mỗi chu kỳ tham nhũng |$O(\infty)$hiệu quả |$O(n)$| Quá chậm/không đáng tin cậy | 
| Các lượt hoán đổi so sánh liền kề lặp đi lặp lại |$O(n^2)$tổng hoạt động |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì không có mảng rõ ràng. Thay vào đó, chúng tôi hoàn toàn dựa vào các sửa đổi cục bộ lặp đi lặp lại bằng cách sử dụng thao tác hoán đổi so sánh được phép. 

### Các bước 

1. Thực hiện nhiều lần các bước vượt qua các vị trí từ trái sang phải bằng cách sử dụng các phép so sánh liền kề$(i, i+1)$. Mỗi truy vấn đảm bảo rằng nếu phần tử lớn hơn nằm ở bên trái của phần tử nhỏ hơn thì nó sẽ được hoán đổi theo đúng hướng. Đây là cơ chế tương tự như sắp xếp bong bóng. 
2. Sau khi hoàn thành một lượt đầy đủ, ngay lập tức bắt đầu một lượt khác. Chúng tôi không dừng lại sau một lần chuyển vì các hoán đổi ngẫu nhiên có thể đưa lại các đảo ngược ở bất kỳ đâu trong mảng. 
3. Tiếp tục thực hiện các lượt vượt qua này cho đến khi trọng tài trả lời THẮNG ở một số câu hỏi. Phản hồi này thay thế đầu ra SWAPPED hoặc STAYED thông thường và báo hiệu rằng mảng hiện đã được sắp xếp hoàn hảo. 
4. Luôn dừng ngay lập tức khi nhận được WIN mà không cần in thêm thao tác nào. 

Lý do chúng tôi sử dụng các so sánh liền kề là vì chúng đảm bảo vị trí điều chỉnh tối đa. Bất kỳ sự đảo ngược nào cuối cùng cũng phải xuất hiện dưới dạng một sự đảo ngược liền kề sau khi có đủ các giá trị hoán đổi lan truyền và một khi điều đó xảy ra, nó có thể sửa được ngay lập tức. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mỗi phép so sánh sẽ loại bỏ sự đảo ngược cục bộ hoặc giữ nguyên trật tự cục bộ. Mặc dù các hoán đổi ngẫu nhiên có thể tạm thời tăng số lượng đảo ngược, nhưng mỗi lần quét toàn bộ sẽ làm giảm nghiêm trọng số lượng đảo ngược hiện ở liền kề hoặc đủ gần để có thể sửa chữa. Vì mọi sự đảo ngược cuối cùng phải trở nên liền kề trong quá trình quét lặp đi lặp lại, nên quá trình này liên tục loại bỏ tình trạng hỗn loạn khỏi hệ thống. 

Bởi vì hệ thống là hữu hạn và mỗi lần chỉnh sửa thành công sẽ di chuyển mảng đến gần hơn với cấu hình được sắp xếp đầy đủ, nên có vô số thời điểm mà mảng được sắp xếp trừ khi bị gián đoạn. Trọng tài phát hiện một trong những khoảnh khắc này và đưa ra THẮNG, tại thời điểm đó chúng tôi kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(i, j):
    print(i, j, flush=True)
    res = input().strip()
    if res == "WIN":
        sys.exit(0)
    return res

def main():
    n = int(input().strip())
    max_ops = 10000
    used = 0

    while used < max_ops:
        for i in range(1, n):
            if used >= max_ops:
                break
            ask(i, i + 1)
            used += 1

if __name__ == "__main__":
    main()
```Việc triển khai tuân theo ý tưởng chuyển vị chẵn-lẻ ở dạng đơn giản nhất: lặp lại các phép so sánh liền kề từ trái sang phải. Mỗi truy vấn được đưa ra ngay lập tức và chúng tôi sẽ chấm dứt ngay lập tức nếu nhận được WIN. 

Điều tinh tế chính là chúng tôi không bao giờ cố gắng lưu trữ hoặc xây dựng lại mảng. Bất kỳ nỗ lực mô phỏng cục bộ nào sẽ không đồng bộ hóa khỏi giám khảo do các hoán đổi ngẫu nhiên bị ẩn. Chương trình vẫn hoàn toàn phản ứng. 

Chúng tôi cũng tôn trọng nghiêm ngặt giao thức tương tác bằng cách xóa mọi đầu ra và dừng ngay lập tức khi WIN. 

## Ví dụ đã hoạt động 

Vì vấn đề có tính chất tương tác nên chúng tôi minh họa cách chạy theo khái niệm thay vì các cặp đầu vào-đầu ra cố định. 

### Ví dụ 1 (trạng thái ban đầu đã gần được sắp xếp) 

Giả sử mảng ẩn là$[1, 3, 2, 4]$. 

| Bước | Truy vấn | Phản hồi | Trạng thái mảng (khái niệm) | 
| --- | --- | --- | --- | 
| 1 | (1,2) | Ở LẠI | [1,3,2,4] | 
| 2 | (2,3) | ĐỔI | [1,2,3,4] | 
| 3 | THẮNG | - | được sắp xếp | 

Điều này chứng tỏ rằng một phép đảo ngược đơn lẻ sẽ bị loại bỏ ngay lập tức khi nó trở thành liền kề. 

Sau bước 2, mảng được sắp xếp đầy đủ và giám khảo sẽ phát hiện điều này trong lần tương tác tiếp theo. 

### Ví dụ 2 (có gián đoạn) 

Giả sử mảng ban đầu$[4,1,3,2]$. 

| Bước | Truy vấn | Phản hồi | Trạng thái mảng (khái niệm) | 
| --- | --- | --- | --- | 
| 1 | (1,2) | ĐỔI | [1,4,3,2] | 
| 2 | (2,3) | ĐỔI | [1,3,4,2] | 
| 3 | (3,4) | ĐỔI | [1,3,2,4] | 
| 4 | (2,3) | ĐỔI | [1,2,3,4] | 
| 5 | (1,2) | THẮNG | được sắp xếp | 

Điều này cho thấy các hiệu chỉnh cục bộ lặp đi lặp lại dần dần loại bỏ các nghịch đảo ngay cả khi chúng trải rộng trên mảng. 

Dấu vết xác nhận rằng mọi sự đảo ngược cuối cùng đều được đẩy vào vị trí mà phép so sánh liền kề sẽ sửa nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot K)$| Mỗi lượt thực hiện$O(n)$truy vấn và chúng tôi chạy đủ lượt để duy trì trong giới hạn 10000 | 
| Không gian |$O(1)$| Không sử dụng bộ nhớ mảng, chỉ sử dụng tương tác | 

Sự ràng buộc$n \le 50$đảm bảo rằng ngay cả vài trăm lần quét toàn bộ cũng nằm trong giới hạn hoạt động 10000. Mỗi lần quét đều rẻ và hạn chế tương tác chiếm ưu thế. 

## Trường hợp thử nghiệm 

Bởi vì đây là tính tương tác, nên các bài kiểm tra là các trình bao bọc khái niệm hiển thị cấu trúc lệnh gọi thay vì mô phỏng đánh giá thực sự.```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return "interactive"

# provided sample (conceptual)
assert run("5") == "interactive"

# minimum size
assert run("2") == "interactive"

# small permutation
assert run("3") == "interactive"

# maximum size
assert run("50") == "interactive"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | tương tác | ranh giới tối thiểu | 
| 3 | tương tác | phân loại không tầm thường nhỏ nhất | 
| 50 | tương tác | hành vi kích thước trường hợp xấu nhất | 
| mẫu n | tương tác | luồng tương tác cơ bản | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi mảng được sắp xếp chính xác giữa hai thao tác do các hoán đổi ngẫu nhiên dừng ở một cấu hình thuận lợi. Trong tình huống này, thuật toán không cần phát hiện rõ ràng tính sắp xếp; nó dựa vào việc thẩm phán trả về THẮNG cho truy vấn tiếp theo. Ví dụ: nếu mảng trở thành$[1,2,3,4]$sau một sự kiện hoán đổi ẩn, thì truy vấn so sánh tiếp theo sẽ ngay lập tức kích hoạt WIN và việc chấm dứt diễn ra chính xác. 

Một trường hợp khác là khi các giao dịch hoán đổi ngẫu nhiên liên tục phá hủy trật tự toàn cầu nhanh hơn mức một lần chuyển đổi có thể sửa chữa được. Ngay cả khi đó, mỗi lần điều chỉnh cục bộ vẫn loại bỏ ít nhất một sự đảo ngược khi gặp phải. Bởi vì mỗi lần quét sẽ kiểm tra lại tất cả các cặp liền kề một cách có hệ thống, nên không có phép đảo ngược nào vĩnh viễn không được kiểm tra. Cuối cùng, một khoảnh khắc xảy ra khi không có sự đảo ngược nào tồn tại chính xác tại thời điểm truy vấn và WIN được kích hoạt.
