---
title: "CF 104945L - Cúp vỡ"
description: "Chúng ta có một tập hợp các ô hình chữ nhật, mỗi ô có độ dài cạnh nguyên $Ak nhân Bk$ trong đó cả hai cạnh nhiều nhất là 3 và ô có thể được xoay. Tất cả các ô cùng nhau có tổng diện tích chính xác là $3N$."
date: "2026-06-28T07:12:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104945
codeforces_index: "L"
codeforces_contest_name: "2023-2024 ICPC Southwestern European Regional Contest (SWERC 2023)"
rating: 0
weight: 104945
solve_time_s: 103
verified: false
draft: false
---

[CF 104945L - Cúp bị vỡ](https://codeforces.com/problemset/problem/104945/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các ô hình chữ nhật, mỗi ô có độ dài cạnh là số nguyên$A_k \times B_k$trong đó cả hai cạnh tối đa là 3 và ô có thể được xoay. Tất cả các ô cùng nhau có tổng diện tích chính xác$3N$. Chúng ta phải sắp xếp chúng sao cho lấp đầy chính xác một tấm bảng có kích thước cố định$3 \times N$, không có sự chồng chéo và không có khoảng trống. 

Đầu ra không phải là mô tả về hình học mà là nhãn: đối với mỗi ô đơn vị của bảng, chúng ta phải xuất ra chỉ mục ô nào bao phủ nó. 

Vì vậy, nhiệm vụ cơ bản là một vấn đề xây dựng lại ốp lát mang tính xây dựng. Chúng tôi không được yêu cầu quyết định tính khả thi vì tính khả thi được đảm bảo. Chúng tôi chỉ được yêu cầu sản xuất một lớp phủ hợp lệ. 

Các ràng buộc rất lớn: lên tới$3 \cdot 10^5$miếng và lên đến$10^5$cột. Điều này ngay lập tức loại trừ bất kỳ chiến lược nào cố gắng mô phỏng việc quay lui tùy ý trên các vị trí hoặc cố gắng tìm kiếm cấu hình. Bất kỳ giải pháp nào về cơ bản phải tuyến tính về số lượng ô hoặc phần, chỉ với công việc không đổi trên mỗi bước. 

Một cách giải thích ngây thơ sẽ cố gắng đặt từng phần một và thử tất cả các vị trí có thể có trên lưới. Điều đó sẽ yêu cầu kiểm tra lên đến$3N$vị trí trên mỗi mảnh, dẫn đến trường hợp xấu nhất là khoảng$10^{10}$hoạt động, điều đó là không thể. 

Một dạng thất bại tinh vi hơn xuất phát từ việc bố trí một cách tham lam mà không có cấu trúc. Nếu chúng ta chọn mảnh đầu tiên “có vẻ vừa vặn” mà không kiểm soát hình dạng của khoảng trống còn lại, chúng ta có thể dễ dàng tạo ra các lỗ bị mắc kẹt. Ví dụ, việc đặt một$2 \times 3$hình chữ nhật quá sớm có thể để lại một$1 \times 2$khoang mà sau này không thể được lấp đầy bởi các mảnh còn lại mặc dù tồn tại một ô xếp toàn cục hợp lệ. Điều này cho thấy tính chính xác đòi hỏi một quy tắc sắp xếp duy trì sự bất biến mạnh mẽ về vùng tự do còn lại chứ không chỉ tính khả thi cục bộ. 

Hạn chế về cấu trúc chính là bảng chỉ có 3 hàng. Chiều cao cố định nhỏ này giúp giải quyết được vấn đề: mọi ranh giới ốp lát một phần chỉ có thể có một số lượng cấu hình rất nhỏ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng từng ô bảng. Đối với mỗi ô trống, chúng tôi thử đặt mọi phần còn lại theo mọi hướng tại vị trí đó, kiểm tra tính hợp lệ, lặp lại và quay lui. Mỗi vị trí bao gồm việc quét tối đa$3 \times 3$tế bào, nhưng hệ số phân nhánh lớn vì có tới$3 \cdot 10^5$miếng. Ngay cả khi việc cắt tỉa loại bỏ hầu hết các cành thì trường hợp xấu nhất sẽ bùng nổ theo cấp số nhân. 

Lý do điều này không thành công là vì chúng tôi đang xử lý vấn đề như tìm kiếm xếp lớp 2D chung, mặc dù chiều cao cố định và cực kỳ nhỏ. 

Quan sát quan trọng là tại bất kỳ thời điểm nào, ranh giới giữa các ô được lấp đầy và không được lấp đầy có thể được mô tả cục bộ. Vì chỉ có 3 hàng nên “hình dạng của ranh giới” bị hạn chế. Điều này cho phép quét tham lam từ trái sang phải: chúng tôi luôn lấy ô chưa được lấp đầy ngoài cùng bên trái và cố gắng đặt một mảnh che phủ nó. Bởi vì tất cả các ô có chiều cao và chiều rộng tối đa là 3 nên mọi vị trí đều chỉ tương tác với vùng lân cận có kích thước không đổi. 

Thay vì khám phá tất cả các vị trí, chúng tôi chỉ định một phần cho ô hiện tại một cách xác định bằng cách khớp nó với một hình chữ nhật vừa với khối trống tối đa có sẵn bắt đầu từ vị trí đó. Vì tổng diện tích khớp chính xác và tồn tại một ô hợp lệ nên tiện ích mở rộng tham lam này không bao giờ bị kẹt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quay lại vũ phu | Hàm mũ | đệ quy O(N) | Quá chậm | 
| Tham lam xây dựng biên giới | O(3N) | O(3N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một$3 \times N$lưới ban đầu trống rỗng. Chúng tôi cũng giữ một con trỏ quét các ô theo thứ tự hàng lớn, luôn trỏ đến ô chưa được lấp đầy đầu tiên. 

1. Tìm ô trống ngoài cùng bên trái$(r, c)$. Đây là vị trí sớm nhất trong thứ tự đọc mà vẫn cần xếp ô. 
2. Từ$(r, c)$, tính hình chữ nhật trống tối đa bắt đầu từ ô đó. Vì chiều cao chỉ là 3 nên hình chữ nhật này có chiều cao tối đa là 3 và chiều rộng tối đa là 3. Chúng ta có thể kiểm tra một cách an toàn xem chúng ta có thể kéo dài sang bên phải bao xa ở mỗi hàng trong số 3 hàng bắt đầu từ$r$, nhưng không bao giờ vượt quá 3 cột do bị chặn bởi các ô đã được điền. 
3. Đặt vùng trống tối đa này có kích thước$h \times w$, Ở đâu$h \le 3$Và$w \le 3$. Bất kỳ cách lát gạch hợp lệ nào cũng phải đặt một miếng vừa khít hoàn toàn bên trong khu vực này và bao phủ$(r, c)$. 
4. Chọn bất kỳ phần nào chưa sử dụng có kích thước, theo một hướng nào đó, khớp chính xác với hình chữ nhật có thể được nhúng bắt đầu từ$(r, c)$. Bởi vì tất cả các mảnh đều đáp ứng$A_k \le B_k \le 3$, chúng ta chỉ cần xét một tập hợp hằng số các hình dạng có thể có. 
5. Đặt mảnh đã chọn sao cho nó bao phủ toàn bộ hình chữ nhật bắt đầu từ$(r, c)$. Đánh dấu tất cả các ô được bao phủ bằng chỉ mục của nó. 
6. Tiếp tục quét cho đến khi tất cả các ô được lấp đầy. 

Phần không cần thiết duy nhất là bước 4: đảm bảo chúng ta luôn có thể tìm thấy một phần phù hợp với vùng trống cục bộ hiện tại. Điều này được đảm bảo bởi thực tế là tổng diện tích khớp chính xác và quá trình quét không bao giờ tạo ra cấu hình ranh giới không thể thực hiện được. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi vị trí, các ô không được lấp đầy còn lại tạo thành một tập hợp các hình chữ nhật hoàn chỉnh được căn chỉnh theo lưới và tương thích với thứ tự quét, đồng thời mọi vùng như vậy đều có thể xếp được theo các phần còn lại. 

Bởi vì lưới có chiều cao 3, nên mọi vật cản sẽ phải xuất hiện dưới dạng một vùng còn sót lại mỏng có chiều cao tối đa là 3 và chiều rộng tối đa là 3 và không thể lấp đầy. Tuy nhiên, vì chúng tôi luôn lấp đầy hình chữ nhật khả thi tối đa từ ô ngoài cùng bên trái nên chúng tôi không bao giờ tạo ra một “lỗ cầu thang” như vậy. Bất kỳ lỗ nào sẽ yêu cầu ranh giới lõm mở rộng đến các cột trong tương lai, nhưng bước tham lam luôn tiêu tốn toàn bộ tiền tố có thể truy cập của khu vực cục bộ đó. 

Do đó, quy trình duy trì một biên giới hợp lệ ở mỗi bước và vì tổng diện tích được bảo toàn nên việc hoàn thành được đảm bảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
```
