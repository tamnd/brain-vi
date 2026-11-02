---
title: "CF 1093G - Truy vấn đa chiều"
description: "Chúng tôi đang làm việc với một tập hợp các điểm trong một không gian có chiều rất thấp, trong đó mỗi điểm có tối đa năm tọa độ. Khoảng cách giữa hai điểm được định nghĩa là tổng chênh lệch tuyệt đối trên mỗi tọa độ, là số liệu Manhattan."
date: "2026-06-15T15:01:33+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1093
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 56 (Rated for Div. 2)"
rating: 2300
weight: 1093
solve_time_s: 221
verified: true
draft: false
---

[CF 1093G - Truy vấn đa chiều](https://codeforces.com/problemset/problem/1093/G) 

**Đánh giá:** 2300 
**Thẻ:** mặt nạ bit, cấu trúc dữ liệu 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một tập hợp các điểm trong một không gian có chiều rất thấp, trong đó mỗi điểm có tối đa năm tọa độ. Khoảng cách giữa hai điểm được định nghĩa là tổng chênh lệch tuyệt đối trên mỗi tọa độ, là số liệu Manhattan. 

Hệ thống phát triển theo thời gian. Bất cứ lúc nào, chúng ta có thể thay thế một điểm tại một chỉ mục nhất định bằng một vectơ tọa độ mới. Giữa các lần cập nhật, chúng tôi được yêu cầu kiểm tra một đoạn liền kề của mảng và xác định khoảng cách Manhattan lớn nhất có thể có giữa hai điểm bất kỳ bên trong đoạn đó. 

Khó khăn chính là cả các bản cập nhật và truy vấn phạm vi đều hoàn toàn trực tuyến, đồng thời kích thước mảng và số lượng thao tác đủ lớn để việc tính toán lại khoảng cách theo cặp bên trong mỗi truy vấn là không khả thi. 

Các ràng buộc buộc chúng tôi phải thực hiện hành vi truy vấn được khấu hao gần như O(log n) hoặc O(1), bởi vì bất kỳ phương pháp nào tính toán lại khoảng cách trong một phạm vi sẽ giảm xuống O(n) cho mỗi truy vấn và dẫn đến khoảng 4e10 thao tác trong trường hợp xấu nhất. 

Một vấn đề tế nhị xuất hiện khi nghĩ về khoảng cách Manhattan đa chiều. Một trực giác ngây thơ là mỗi chiều có thể được xử lý độc lập, nhưng cặp giá trị tuyệt đối phối hợp với nhau theo cách ngăn chặn sự phân rã trực tiếp trừ khi chúng ta sử dụng một phép biến đổi cấu trúc. 

Một dạng lỗi phổ biến khác là giả định rằng khoảng cách tối đa trong một đoạn luôn xuất phát từ các giá trị cực trị của từng tọa độ một cách độc lập. Điều này là sai vì cùng một điểm phải được sử dụng nhất quán trên tất cả các chiều chứ không phải độc lập trên mỗi tọa độ. 

Ví dụ: xét điểm (0, 100) và (100, 0). Mỗi tọa độ riêng lẻ đều có cả hai điểm cực trị, nhưng khoảng cách tối đa của Manhattan đến từ việc ghép hai điểm này chứ không phải từ việc trộn các tọa độ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mọi truy vấn loại 2, hãy tính khoảng cách giữa mọi cặp điểm trong phạm vi. Điều này đúng vì nó đánh giá rõ ràng mọi khả năng. Tuy nhiên, mỗi truy vấn sẽ có chi phí O((r-l)^2 * k), trong trường hợp xấu nhất sẽ suy biến thành O(n^2 k). Với tối đa 2e5 truy vấn, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là khoảng cách Manhattan với chiều k nhỏ có thể được biến đổi bằng cách sử dụng các mẫu dấu. Đối với bất kỳ vectơ dấu cố định nào (mỗi tọa độ +1 hoặc -1), chúng ta có thể viết lại điểm x dưới dạng phép chiếu vô hướng: 

s(x) = tổng_i ký_i * x_i 

Bây giờ hãy xem xét hai điểm a và b. Với mọi vectơ dấu, ta có: 

s(a) - s(b) = sum_i sign_i (a_i - b_i) 

Nếu chúng ta chọn sign_i = sign(a_i - b_i), biểu thức này sẽ trở thành chính xác khoảng cách Manhattan giữa a và b. Điều này dẫn đến một nhận dạng cổ điển: khoảng cách Manhattan bằng mức tối đa trên tất cả các cấu hình dấu 2^k của sự khác biệt tuyệt đối trong các phép chiếu tuyến tính này. 

Vì vậy, thay vì làm việc trực tiếp với k chiều, chúng tôi giảm vấn đề xuống việc duy trì, đối với mỗi mẫu dấu trong số 2^k, giá trị tối đa và tối thiểu của phép chiếu trên một đoạn. Cặp tốt nhất cho một mẫu cố định chỉ đơn giản là max(s(x)) - min(s(x)). 

Vì k 5 nên chúng ta chỉ có tối đa 32 mẫu dấu, đủ nhỏ để duy trì độc lập. 

Mỗi điểm đóng góp một giá trị cho mỗi mẫu và mỗi bản cập nhật sẽ ảnh hưởng đến tất cả các mẫu. Mỗi truy vấn phạm vi giảm xuống mức tối đa là 32 truy vấn phân đoạn, mỗi truy vấn trả về giá trị tối thiểu và tối đa. 

Cấu trúc này được xử lý một cách tự nhiên bằng cách sử dụng cây phân đoạn trên mỗi mẫu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 k) mỗi truy vấn | O(nk) | Quá chậm | 
| Mẫu biển báo + cây phân đoạn | O((n + q) * 2^k log n) | O(2^k n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Liệt kê tất cả các mặt nạ dấu từ 0 đến 2^k - 1. Mỗi bit biểu thị tọa độ được cộng hay trừ. Điều này đảm bảo chúng tôi bao gồm tất cả các phép biến đổi cần thiết để tái tạo lại khoảng cách Manhattan. 
2. Đối với mỗi mặt nạ, xác định giá trị được chuyển đổi cho mỗi điểm dưới dạng tổng tọa độ có dấu của nó. Điều này làm giảm một điểm vectơ thành một điểm vô hướng trong khi vẫn giữ được thông tin khoảng cách dưới mặt nạ đó. 
3. Xây dựng cây phân đoạn cho mỗi mặt nạ lưu trữ hai giá trị cho mỗi phân đoạn: giá trị chuyển đổi tối thiểu và tối đa trong phân đoạn đó. Hai giá trị này là đủ vì bất kỳ khoảng cách cặp nào dưới mặt nạ đó đều được xác định bằng max trừ min. 
4. Khi cập nhật một điểm, hãy tính lại giá trị đã chuyển đổi của nó dưới tất cả các mặt nạ và cập nhật các vị trí lá tương ứng trong mỗi cây phân đoạn. Điều này đảm bảo tính nhất quán trên tất cả các dự đoán. 
5. Khi trả lời một truy vấn phạm vi, đối với mỗi mặt nạ, hãy truy vấn độc lập cây phân đoạn của nó về giá trị tối thiểu và tối đa trong khoảng thời gian đó. Tính câu trả lời của ứng viên theo giá trị tối đa trừ tối thiểu. 
6. Tận dụng tối đa tất cả các mặt nạ, vì khoảng cách thực của Manhattan tương ứng với cấu hình biển báo tốt nhất trong số tất cả các mặt nạ. 

### Tại sao nó hoạt động 

Thuật toán dựa trên nhận dạng rằng khoảng cách Manhattan giữa hai điểm bằng giá trị lớn nhất trên tất cả các phép gán dấu của hiệu của các phép chiếu có dấu. Mỗi cây phân đoạn đảm bảo rằng đối với một mẫu dấu cố định, chúng ta luôn biết các giá trị hình chiếu cực trị trong bất kỳ khoảng nào. Vì bất kỳ cặp nào trong khoảng phải được ghi lại dưới ít nhất một mẫu dấu hiệu phù hợp với sự khác biệt về tọa độ của chúng, mức tối đa trên tất cả các mặt nạ sẽ tái tạo lại đường kính Manhattan chính xác của đoạn. 

Không có cặp ứng cử viên nào bị bỏ sót vì mỗi cặp tạo ra một mẫu dấu tối ưu duy nhất để chuyển đổi hiệu tọa độ của nó thành tổng không âm trong phép chiếu đó. 

## Giải pháp Python```
PythonRun
```Giải pháp này xây dựng 2^k cây phân đoạn độc lập, mỗi cây lưu trữ một hình chiếu có dấu khác nhau của các điểm. Mỗi bản cập nhật sẽ tính toán lại tất cả các phép chiếu cho một điểm duy nhất. Mỗi truy vấn tổng hợp sự khác biệt tốt nhất trên tất cả các phép chiếu bằng cách trích xuất mức tối thiểu và tối đa trong khoảng thời gian. 

Một điểm tinh tế chung là cấu trúc cây phân đoạn giống nhau được sử dụng lại cho cả truy vấn tối thiểu và tối đa. Việc trộn các cây riêng biệt cho mỗi cây sẽ dư thừa và chậm hơn, do đó cả hai giá trị đều được lưu trữ trong một nút. 

Một chi tiết quan trọng khác là duy trì lập chỉ mục dựa trên 0 một cách nhất quán trên các bản cập nhật và truy vấn, vì cây phân đoạn được xây dựng trên biểu diễn mảng. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một phiên bản 2D nhỏ để minh họa cách mặt nạ ký hiệu tương tác. 

đầu vào:```

```Chúng tôi xem xét bốn mặt nạ. 

| mặt nạ | điểm 1 | điểm 2 | điểm 3 | phút | tối đa | khác biệt | 
| --- | --- | --- | --- | --- | --- | --- | 
| ++ | 3 | 5 | 4 | 3 | 5 | 2 | 
| +- | -1 | -1 | 4 | -1 | 4 | 5 | 
| -+ | 1 | 1 | -4 | -4 | 1 | 5 | 
| -- | -3 | -5 | -4 | -5 | -3 | 2 | 

Tối đa là 5, tương ứng với khoảng cách giữa (2,3) và (4,0). 

Điều này cho thấy cặp đúng xuất hiện dưới một cấu hình dấu hiệu cụ thể thay vì các cực trị về tọa độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) · 2^k · log n) | Mỗi bản cập nhật chạm vào tất cả các mặt nạ, mỗi truy vấn sẽ kiểm tra tất cả các mặt nạ bằng các thao tác cây phân đoạn | 
| Không gian | O(n · 2^k) | Mỗi mặt nạ lưu trữ một cây phân đoạn đầy đủ trên n phần tử | 

Ràng buộc k 5 giữ 2^k 32, điều này làm cho phương pháp này đủ nhanh ngay cả đối với các thao tác 2e5. Hệ số logarit từ cây phân đoạn vẫn được chấp nhận dưới 6 giây. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn điểm đơn | 0 | không có cặp tồn tại có hiệu lực | 
| hai điểm được cập nhật liên tục | đúng Manhattan | cập nhật tính đúng đắn | 
| k=1 vỏ cạnh | sự khác biệt tuyệt đối | giảm độ đúng | 
| cập nhật và truy vấn xen kẽ | kết quả nhất quán | kiên trì trong các hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k = 1. Trong tình huống này, vấn đề suy biến thành việc duy trì chênh lệch tối đa trong một phạm vi số nguyên được cập nhật. Thuật toán vẫn hoạt động vì chỉ có hai mặt nạ: + và -. Cả hai đều tạo ra cấu trúc chênh lệch tuyệt đối giống nhau và cây phân đoạn theo dõi chính xác giá trị tối thiểu và tối đa. 

Một trường hợp cạnh khác phát sinh khi tất cả các điểm giống hệt nhau ngoại trừ một lần cập nhật tọa độ. Cây phân đoạn đảm bảo rằng chỉ các phép chiếu bị ảnh hưởng thay đổi, ngăn cực đại cũ ảnh hưởng đến các mặt nạ không liên quan. 

Cuối cùng, các cập nhật và truy vấn xen kẽ trong trường hợp xấu nhất trên tất cả các chỉ mục nhấn mạnh tính nhất quán của các cập nhật trên mỗi mặt nạ. Vì mỗi bản cập nhật tính toán lại tất cả các phép chiếu 2^k một cách độc lập nên không xảy ra hiện tượng nhiễu mặt nạ chéo, duy trì tính chính xác ngay cả trong các chuỗi đối nghịch.
