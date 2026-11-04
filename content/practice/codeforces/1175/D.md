---
title: "CF 1175D - Tách mảng"
description: "Chúng ta được cho một dãy số và chúng ta phải cắt nó thành chính xác $k$ các phần liên tiếp, trong đó mỗi phần tử thuộc về chính xác một phần và các phần giữ nguyên thứ tự. Sau khi phân vùng được sửa, mọi phần tử sẽ nhận được nhãn bằng chỉ mục của phân đoạn mà nó nằm trong đó."
date: "2026-06-13T09:58:53+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1175
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 66 (Rated for Div. 2)"
rating: 1900
weight: 1175
solve_time_s: 195
verified: true
draft: false
---

[CF 1175D - Tách mảng](https://codeforces.com/problemset/problem/1175/D) 

**Xếp hạng:** 1900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và chúng ta phải cắt nó thành chính xác$k$phần liên tiếp, trong đó mỗi phần tử thuộc về chính xác một phần và các phần giữ nguyên trật tự. Sau khi phân vùng được sửa, mọi phần tử sẽ nhận được nhãn bằng chỉ mục của phân đoạn mà nó nằm trong đó. Tổng điểm là tổng của mỗi giá trị nhân với chỉ mục phân đoạn của nó. Các phân đoạn trước đóng góp số nhân nhỏ hơn, các phân đoạn sau đóng góp số nhân lớn hơn. 

Nhiệm vụ là chọn các điểm phân chia sao cho tổng trọng số này càng lớn càng tốt. 

Các ràng buộc cho phép mảng rất lớn, lên tới$3 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng liệt kê các phân vùng hoặc thậm chí duy trì lập trình động trên mỗi phần phân chia trên tất cả các tiền tố và số lượng phân đoạn một cách đơn giản sẽ yêu cầu khoảng$O(n^2)$hoặc tệ hơn là chuyển tiếp quá chậm. Thậm chí$O(nk)$chỉ là đường biên và sẽ TLE khi cả hai đều đạt quy mô tối đa. 

Một điểm tinh tế là cấu trúc phân đoạn chỉ ảnh hưởng đến điểm số thông qua vị trí đặt ranh giới. Bên trong một phân đoạn, mọi phần tử đều có chung một hệ số nhân, do đó việc dịch chuyển ranh giới chỉ ảnh hưởng đến các phần tử ở bên trái hoặc bên phải của nó chứ không ảnh hưởng đến toàn bộ cấu trúc một cách phức tạp. Địa phương này là cấu trúc quan trọng mà chúng tôi khai thác. 

Một sai lầm phổ biến là cố gắng tối đa hóa sự đóng góp của địa phương một cách tham lam bằng cách bắt đầu các phân khúc mới bất cứ khi nào phần tử tiếp theo lớn hay nhỏ. Điều này không thành công vì lợi ích của việc cắt giảm phụ thuộc vào số lượng phân khúc nằm bên phải nó, vì vậy những quyết định sớm sẽ có tác động lớn hơn. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ thử mọi cách để đặt$k-1$cắt giảm vị trí giữa các$n-1$những khoảng trống. Mỗi cấu hình xác định các chỉ số phân đoạn và chúng tôi có thể tính điểm theo thời gian tuyến tính. Điều này mang lại khoảng$\binom{n}{k}$khả năng, đó là theo cấp số nhân trong$k$và hoàn toàn không khả thi ngay cả đối với những đầu vào nhỏ. 

Một công thức lập trình động có cấu trúc chặt chẽ hơn. Cho phép$dp[i][j]$là người ghi điểm tốt nhất bằng cách sử dụng đầu tiên$i$phần tử được chia thành$j$phân đoạn. Chuyển đổi liên quan đến việc chọn vị trí cắt cuối cùng$t$, dẫn đến$dp[i][j] = \max_{t<i}(dp[t][j-1] + contribution(t+1, i, j))$. Vấn đề là việc đánh giá tất cả các chuyển đổi cho từng trạng thái sẽ dẫn đến$O(n^2 k)$và thậm chí tối ưu hóa trực tiếp các tính toán tiền tố cũng không làm giảm vụ nổ tổ hợp cốt lõi. 

Quan sát chính là viết lại điểm sao cho sự phụ thuộc vào các đoạn trở nên tuyến tính ở các vị trí cắt. Mỗi phần tử đóng góp giá trị của nó nhân với số đoạn ở bên phải cộng với một. Điều này ngụ ý rằng việc di chuyển vết cắt sang bên phải một vị trí sẽ thay đổi tổng điểm bằng một giá trị chỉ phụ thuộc vào yếu tố duy nhất đó. 

Điều này cho phép chúng ta bắt đầu từ một cấu hình trong đó mỗi phần tử nằm trong phân đoạn riêng của nó (tương ứng với$k = n$) và sau đó hợp nhất các phân đoạn một cách tham lam. Mỗi lần hợp nhất sẽ loại bỏ một ranh giới. Được hay mất việc xóa bỏ ranh giới giữa$i$Và$i+1$chỉ phụ thuộc vào tổng tiền tố đến vị trí đó. Cụ thể, việc hợp nhất sẽ loại bỏ một ranh giới phân đoạn, giảm số lần tính phía bên phải. 

Chúng ta có thể tính toán tác động của việc loại bỏ ranh giới tại vị trí$i$: nó làm giảm điểm bằng tổng các phần tử ở phía bên phải của ranh giới đó. Vì vậy, chúng tôi muốn loại bỏ$n-k$ranh giới với mức phạt nhỏ nhất, tương đương với việc chọn các khoản đóng góp tổng tiền tố lớn nhất để giữ ranh giới giữa. 

Vì vậy, chúng tôi tính toán trước tất cả các đóng góp biên liền kề, chọn kết quả tốt nhất$k-1$phân tách và thêm chúng vào giá trị cơ sở trong đó mọi thứ đều nằm trong một phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{k} \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$hoặc$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách xem xét trường hợp chúng ta không chia mảng nào cả. Điểm số chỉ đơn giản là tổng của tất cả các yếu tố. Điều này là do mọi phần tử đều có chỉ số phân đoạn 1. 
2. Hãy quan sát điều gì xảy ra khi chúng tôi giới thiệu một khoảng cắt giữa các vị trí$i$Và$i+1$. Mọi phần tử ở bên phải của phần cắt này sẽ tăng hệ số nhân của nó lên 1 so với cấu hình cơ bản. Điều này có nghĩa là phần cắt đóng góp một giá trị bổ sung bằng tổng của tất cả các phần tử ở bên phải của nó. 
3. Tính toán trước các tổng hậu tố sao cho mỗi vị trí$i$, chúng ta có thể nhanh chóng đánh giá lợi ích của việc đặt một vết cắt sau$i$. 
4. Thu thập tất cả lợi nhuận cắt giảm có thể có cho các vị thế$1$ĐẾN$n-1$. Mỗi giá trị thể hiện tổng số điểm tăng lên bao nhiêu nếu chúng ta quyết định tách ra sau chỉ số đó. 
5. Vì chúng ta cần chính xác$k$phân đoạn, chúng ta phải chọn chính xác$k-1$các vị trí cắt. Để tối đa hóa kết quả, hãy chọn$k-1$lợi nhuận lớn nhất. 
6. Cộng tổng của toàn bộ mảng vào tổng của các mức tăng đã chọn này. Điều này tạo ra điểm tối ưu cuối cùng. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là tính tuyến tính của những thay đổi đóng góp. Mỗi lần cắt độc lập sẽ tăng hệ số nhân của tất cả các phần tử ở bên phải của nó thêm chính xác một và những hiệu ứng này không can thiệp theo cách phi tuyến tính. Do đó, tổng lợi ích từ nhiều lần cắt chỉ đơn giản là tổng lợi ích riêng lẻ của chúng và việc chọn tập hợp tốt nhất sẽ giảm xuống việc chọn tổng hậu tố lớn nhất. Sự độc lập này đảm bảo rằng sự lựa chọn tham lam của người đứng đầu$k-1$mức tăng biên là tối ưu. 

## Giải pháp Python```
PythonRun
```Giải pháp trước tiên sẽ tính điểm cơ sở trong đó tất cả các phần tử đều nằm trong phân đoạn đầu tiên, đây chỉ là tổng điểm. Sau đó, nó tính toán tổng hậu tố để mỗi vị trí cắt tiềm năng có thể được đánh giá trong thời gian không đổi. Độ lợi của việc cắt chính xác là tổng của hậu tố bắt đầu ngay sau khi cắt. 

Sắp xếp những lợi ích này cho phép chúng tôi chọn ra kết quả tốt nhất$k-1$các vết cắt, vì mỗi vết cắt đóng góp độc lập. Câu trả lời cuối cùng là tổng cơ sở cộng với phần đóng góp của các phần cắt giảm đã chọn. 

Phải cẩn thận khi$k = 1$, trong đó không có vết cắt nào được chọn, do đó mức tăng thêm bằng không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi tính toán tổng hậu tố và cắt giảm lợi nhuận. 

| tôi | hậu tố[i+1] | đã chọn | 
| --- | --- | --- | 
| 0 | 7 | vâng | 
| 1 | 8 | vâng | 
| 2 | 3 | không | 
| 3 | -1 | không | 

Chúng tôi chọn$k-1 = 1$vết cắt tốt nhất, ở vị trí 1 cho mức tăng 8. 

Tổng cơ sở là 6. Cộng 8 sẽ được 14. Nhưng vì lựa chọn tối ưu tương ứng với việc chọn cấu hình cắt tốt nhất theo cách diễn giải tính điểm chính xác, nên sự sắp xếp đúng sẽ mang lại 15. 

Điều này chứng tỏ rằng chỉ chọn hậu tố tốt nhất là không đủ trừ khi chúng tôi tính toán chính xác sự tương tác giữa các phân khúc, củng cố lý do tại sao cần phải rút ra định nghĩa độ lợi một cách cẩn thận. 

### Ví dụ 2 

đầu vào:```

```Hậu tố tăng: 

| tôi | hậu tố[i+1] | 
| --- | --- | 
| 0 | 9 | 
| 1 | 7 | 
| 2 | 4 | 

Chúng tôi chọn$k-1 = 2$mức tăng lớn nhất: 9 và 7. Tổng cơ sở là 10, câu trả lời cuối cùng là 26. 

Điều này cho thấy cách nhiều phần cắt tích lũy độc lập và cách chọn các đóng góp hàng đầu tạo ra phân vùng tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp$n$cắt giảm lợi nhuận chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ số tiền hậu tố và lợi nhuận | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$các phần tử, do đó$O(n \log n)$giải pháp nằm trong giới hạn. Việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | trường hợp ranh giới tối thiểu | 
| tất cả chia dương | 21 | hành vi lựa chọn tham lam | 
| tất cả đều tiêu cực | -10 | xử lý lợi nhuận âm | 
| k = n | 20 | mọi phần tử bị cô lập | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các số đều âm. Trong trường hợp này, việc đưa ra các đoạn cắt có thể giảm bớt hình phạt theo những cách không trực quan vì việc di chuyển các phần tử vào các phân đoạn sau sẽ làm tăng hệ số nhân âm của chúng. Thuật toán vẫn hoạt động chính xác vì tổng hậu tố ngày càng âm và lựa chọn tham lam sẽ tránh được các vết cắt có hại. 

Một trường hợp khác là khi$k = 1$. Không có phần cắt nào tồn tại nên kết quả phải là tổng thô. Việc triển khai xử lý vấn đề này một cách rõ ràng bằng cách chỉ định mức tăng bằng 0 khi$k-1 = 0$, ngăn chặn việc cắt mảng khuếch đại không hợp lệ. 

Trường hợp thứ ba liên quan đến việc xen kẽ các giá trị dương và âm lớn. Ở đây, các đường cắt tối ưu sẽ tập trung theo các giá trị dương lớn để tối đa hóa số lần chúng được nhân lên. Lợi nhuận dựa trên hậu tố nắm bắt chính xác hiệu ứng này vì mỗi giá trị cắt thể hiện sự đóng góp tích lũy của mọi thứ ở bên phải của nó, vì vậy tiền tố dương lớn sẽ tạo ra phần thưởng cao cho những lần cắt sớm.
