---
title: "CF 104095A - \u73ed\u59d4\u7ade\u9009"
description: "Mỗi học sinh hoặc cạnh tranh cho đúng một vị trí hoặc không ai trong số họ quan trọng đối với một vị trí nhất định. Đối với mọi vị trí, chúng ta phải xem xét tất cả học sinh đã ứng tuyển vào vị trí đó và chọn ra người có số phiếu bầu cao nhất."
date: "2026-07-02T02:19:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104095
codeforces_index: "A"
codeforces_contest_name: "2020 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 104095
solve_time_s: 161
verified: true
draft: false
---

[CF 104095A - \u73ed\u59d4\u7ade\u9009](https://codeforces.com/problemset/problem/104095/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh hoặc cạnh tranh cho đúng một vị trí hoặc không ai trong số họ quan trọng đối với một vị trí nhất định. Đối với mọi vị trí, chúng ta phải xem xét tất cả học sinh đã ứng tuyển vào vị trí đó và chọn ra người có số phiếu bầu cao nhất. Nếu nhiều học sinh đạt được số phiếu bầu tối đa như nhau thì sự cân bằng sẽ được giải quyết bằng cách chọn chỉ số học sinh nhỏ nhất. 

Đầu vào đưa ra một danh sách học sinh, trong đó mỗi học sinh tuyên bố một vị trí mục tiêu và số phiếu bầu. Nhiệm vụ là tái cấu trúc cho mọi vị trí từ$1$ĐẾN$m$, học sinh nào giành được vị trí đó sau khi áp dụng quy tắc “số phiếu tối đa, sau đó chỉ số nhỏ nhất”. 

Giới hạn rất nhỏ:$n \le 51$Và$m \le 12$. Điều này loại bỏ bất kỳ mối quan tâm về hiệu quả. Thậm chí một$O(nm)$hoặc$O(n^2)$mô phỏng nhanh một cách tầm thường. Yêu cầu thực sự duy nhất là tính chính xác của việc phân nhóm và liên kết. 

Trường hợp thất bại phổ biến nhất là do quên thứ tự ràng buộc hoặc không phân nhóm học sinh theo vị trí một cách chính xác. Ví dụ: nếu hai học sinh nhắm mục tiêu vào cùng một vị trí với số phiếu bầu bằng nhau, việc chọn dữ liệu đầu vào sau thay vì chỉ mục nhỏ hơn sẽ tạo ra câu trả lời sai ngay cả khi tất cả các giá trị đều được xử lý. 

Một vấn đề tế nhị khác là giả định rằng các vị trí là độc lập nhưng vô tình trộn lẫn các chỉ số giữa các vị trí do các biến được chia sẻ hoặc không đặt lại mức tối đa cho mỗi vị trí. 

## Phương pháp tiếp cận 

Cấu trúc của vấn đề đã là cách tiếp cận tối ưu. Mỗi vị trí đều độc lập, vì vậy giải pháp tự nhiên là nhóm tất cả các ứng viên theo vị trí họ đã chọn và tính mức tối đa đơn giản. 

Đối với mỗi vị trí, một cách diễn giải thô bạo sẽ quét tất cả học sinh và chọn ra ứng viên tốt nhất. Điều này có tác dụng vì mỗi quyết định vị trí đều độc lập và chỉ yêu cầu quét tuyến tính. Chi phí là$O(nm)$hoạt động, vì đối với mỗi$m$vị trí chúng tôi kiểm tra tất cả$n$sinh viên. Với những ràng buộc nhất định, điều này đã ở mức tối thiểu và hiệu quả. 

Quan sát quan trọng là không có sự tương tác nào tồn tại giữa các vị trí. Một học sinh thuộc chính xác một nhóm, vì vậy chúng tôi không bao giờ cần sắp xếp hoặc so khớp toàn cục. Toàn bộ vấn đề giảm xuống mức lựa chọn tối đa lặp đi lặp lại với khóa từ điển là$(t_i, -i)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force trên mỗi vị trí |$O(nm)$|$O(1)$thêm | Đã chấp nhận | 
| Nhóm + vé đơn |$O(n + m)$|$O(m)$| Đã chấp nhận | 

Cả hai đều giống nhau trong thực tế; phiên bản được nhóm chỉ làm cho cấu trúc rõ ràng hơn. 

## Hướng dẫn thuật toán 

1. Tạo một mảng`best_id`kích thước$m$khởi tạo thành$0$, đại diện cho người chiến thắng hiện tại cho mỗi vị trí. Cũng duy trì`best_score`khởi tạo thành$0$cho từng vị trí. Thiết lập này mã hóa bất biến mà mỗi vị trí luôn lưu trữ ứng cử viên tốt nhất hiện tại. 
2. Đọc từng học sinh$i$từ$1$ĐẾN$n$, với vị trí đã chọn$c_i$và phiếu bầu$t_i$. 
3. Đối với vị trí$c_i$, so sánh$t_i$với số điểm tốt nhất hiện tại được lưu trữ trong`best_score[c_i]`. 
4. Nếu$t_i$lớn hơn điểm tốt nhất hiện tại, hãy thay thế cả hai`best_score[c_i]`Và`best_id[c_i]`với$t_i$Và$i$. Điều này đảm bảo rằng vị trí này sẽ giữ được ứng cử viên có số phiếu bầu cao nhất cho đến nay. 
5. Nếu$t_i$bằng điểm tốt nhất hiện tại, so sánh chỉ số học sinh. Nếu như$i$nhỏ hơn chỉ mục tốt nhất được lưu trữ, hãy thay thế chỉ mục chiến thắng được lưu trữ. Điều này thực thi quy tắc ràng buộc. 
6. Sau khi xử lý tất cả học sinh, xuất ra`best_id[1]`bởi vì`best_id[m]`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình xử lý, mỗi vị trí đều lưu trữ ứng viên tốt nhất trong số tất cả học sinh được tìm thấy cho đến nay cho vị trí đó theo thứ tự từ điển theo thứ tự từ điển.$(t_i, -i)$. Mỗi học sinh mới hoặc hoàn toàn tốt hơn, hoàn toàn kém hơn hoặc bị ràng buộc về số phiếu bầu, và trong trường hợp hòa chỉ có chỉ số nhỏ nhất tồn tại. Vì mỗi học sinh được xử lý chính xác một lần và các phép so sánh là các cập nhật đơn điệu nên giá trị được lưu trữ cuối cùng phải là giá trị cực đại toàn cục cho mỗi vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    best_score = [0] * (m + 1)
    best_id = [0] * (m + 1)

    for i in range(1, n + 1):
        c, t = map(int, input().split())
        if t > best_score[c]:
            best_score[c] = t
            best_id[c] = i
        elif t == best_score[c] and i < best_id[c]:
            best_id[c] = i

    print(*best_id[1:])

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai mảng được lập chỉ mục theo vị trí. Một cửa hàng lưu trữ số phiếu bầu tốt nhất được thấy cho đến nay và cửa hàng còn lại lưu trữ chỉ số sinh viên tương ứng. Logic cập nhật mã hóa trực tiếp quy tắc lựa chọn và đầu ra cuối cùng chỉ liệt kê những người chiến thắng trên mỗi vị trí theo thứ tự. 

Một điểm tinh tế là khởi tạo: thiết lập`best_score`ĐẾN$0$hợp lệ vì số phiếu bầu được đảm bảo ít nhất$1$. Một chi tiết nữa đó là`best_id`bắt đầu lúc$0$, điều này có hiệu quả vì bất kỳ chỉ số sinh viên thực tế nào ít nhất$1$, vì vậy ứng cử viên hợp lệ đầu tiên luôn ghi đè lên nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi theo dõi trạng thái của mỗi vị trí. 

| Sinh viên | Vị trí | Bình chọn | điểm_score tốt nhất[1] | tốt nhất_id[1] | điểm_score tốt nhất[2] | best_id[2] | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 5 | 1 | 0 | 0 | 
| 2 | 1 | 3 | 5 | 1 | 0 | 0 | 
| 3 | 2 | 4 | 5 | 1 | 4 | 3 | 

Đầu ra cuối cùng là`1 3`. 

Điều này xác nhận rằng các ứng cử viên yếu hơn sau này không ghi đè lên những ứng cử viên mạnh hơn. 

### Ví dụ 2 

đầu vào:```

```| Sinh viên | Vị trí | Bình chọn | điểm_score tốt nhất[1] | tốt nhất_id[1] | điểm_score tốt nhất[2] | best_id[2] | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 7 | 0 | 0 | 7 | 1 | 
| 2 | 2 | 7 | 0 | 0 | 7 | 1 | 
| 3 | 1 | 5 | 5 | 3 | 7 | 1 | 
| 4 | 1 | 5 | 5 | 3 | 7 | 1 | 

Đầu ra cuối cùng là`3 1`. 

Điều này cho thấy sự ngang nhau: đối với vị trí 1, học sinh thứ 3 và 4 ngang nhau về số phiếu bầu, do đó chỉ số 3 nhỏ hơn vẫn được giữ nguyên. Đối với vị trí 2, học sinh 1 vẫn giữ nguyên vì các mục bằng nhau sau này không thay thế chỉ số nhỏ hơn trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi học sinh được xử lý một lần với các cập nhật liên tục | 
| Không gian |$O(m)$| Mảng lưu trữ ứng viên tốt nhất cho mỗi vị trí | 

Những hạn chế$n \le 51$Và$m \le 12$thấp hơn nhiều so với giới hạn thông thường, do đó, ngay cả những giải pháp được tối ưu hóa kém hơn cũng có thể dễ dàng vượt qua. Phương pháp được lựa chọn là tối ưu về cấu trúc và hệ số không đổi. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 1 5 | tính đúng đắn cơ bản và nhóm | 
| vị trí đơn | 1 | tổng hợp và liên kết | 
| hộp đựng cà vạt | 1 3 | quy tắc chỉ số nhỏ nhất | 
| trường hợp nhận dạng | 1 2 3 | vị trí độc lập | 

## Vỏ cạnh 

Trường hợp quan trọng là khi nhiều sinh viên cạnh tranh cho cùng một vị trí với số phiếu bầu giống hệt nhau. Thuật toán xử lý vấn đề này bằng cách chỉ so sánh các chỉ số khi số phiếu bầu bằng nhau, đảm bảo chỉ số nhỏ nhất được giữ nguyên. 

Một trường hợp khác là khi một vị trí chỉ nhận được một ứng cử viên. Việc khởi tạo bằng 0 đảm bảo ứng cử viên đầu tiên luôn trở thành người chiến thắng. 

Trường hợp góc cuối cùng là khi các ứng cử viên cho các vị trí khác nhau xen kẽ theo thứ tự đầu vào. Vì mỗi bản cập nhật chỉ chạm vào chỉ mục vị trí của chính nó nên không xảy ra nhiễu giữa các vị trí, tự động duy trì tính chính xác.
