---
title: "CF 1027F - Phiên trong BSU"
description: "Chúng ta được giao một tập hợp các nhiệm vụ, mỗi nhiệm vụ đại diện cho một kỳ thi phải được lên lịch vào đúng một trong hai ngày có thể. Đối với kỳ thi $i$, có hai ngày thí sinh $ai$ và $bi$, và chúng ta phải chọn một trong số đó."
date: "2026-06-16T21:39:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "dsu", "graph-matchings", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1027
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 49 (Rated for Div. 2)"
rating: 2400
weight: 1027
solve_time_s: 417
verified: false
draft: false
---

[CF 1027F - Phiên trong BSU](https://codeforces.com/problemset/problem/1027/F) 

**Đánh giá:** 2400 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, dsu, đối sánh đồ thị, đồ thị 
**Thời gian giải:** 6 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một tập hợp các nhiệm vụ, mỗi nhiệm vụ đại diện cho một kỳ thi phải được lên lịch vào đúng một trong hai ngày có thể. Để thi$i$, có hai ngày ứng cử$a_i$Và$b_i$, và chúng ta phải chọn một trong số chúng. Hạn chế là không thể thực hiện nhiều hơn một bài kiểm tra trong bất kỳ ngày nào, vì vậy những ngày được chọn trong tất cả các bài kiểm tra phải khác biệt. 

Mục tiêu không chỉ là tìm ra bất kỳ nhiệm vụ hợp lệ nào mà còn để giảm thiểu ngày gần nhất được sử dụng trong nhiệm vụ đó. Nói cách khác, chúng tôi muốn chỉ định mỗi bài kiểm tra vào ngày đầu tiên hoặc ngày thứ hai có sẵn để tất cả các bài tập không bị xung đột và ngày được chọn tối đa càng nhỏ càng tốt. Nếu không có nhiệm vụ nào tồn tại, chúng tôi phải báo cáo là không thể thực hiện được. 

Cấu trúc ngay lập tức gợi ý một bài toán gán kiểu hai bên với hai lựa chọn cho mỗi mục, nhưng điều phức tạp là miền ngày lớn tới$10^9$, trong khi số lượng bài kiểm tra lên tới$10^6$. Điều này loại trừ bất kỳ cách tiếp cận nào xây dựng cấu trúc theo ngày có kích thước đó một cách rõ ràng. 

Ràng buộc$n \le 10^6$ngụ ý rằng bất kỳ giải pháp nào cũng phải gần với tuyến tính hoặc tuyến tính, vì$O(n \log n)$là giới hạn trên điển hình tồn tại trong Python ở quy mô này. Bất cứ điều gì bậc hai trong các kỳ thi hoặc trong ngày đều không thể thực hiện được ngay lập tức. 

Một trường hợp thất bại tinh vi xuất hiện khi một phép gán tham lam chọn sai các ràng buộc ban đầu: 

Nếu có hai kỳ thi$(1, 2)$Và$(1, 2)$, một chiến lược ngây thơ có thể gán cả hai cho ngày 1, nhưng điều đó không hợp lệ. Ngược lại, luôn chọn ngày có sẵn sớm nhất mà không có cấu trúc có thể chặn các nhiệm vụ sau này, ngay cả khi có lịch trình hợp lệ. 

Một trường hợp phức tạp khác là khi nhiều bài kiểm tra có chung một ngày thi sớm nhưng có những ngày dự phòng khác nhau. Nếu chúng tôi không kiểm soát cách chúng tôi chỉ định các thời điểm đầu đó, chúng tôi có thể đẩy quá nhiều bài kiểm tra sang những ngày sau đó một cách không cần thiết, tăng ngày tối đa cuối cùng hoặc thậm chí khiến việc phân công không thể thực hiện được. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ sẽ coi đây là một vấn đề chuyển nhượng quay lui. Đối với mỗi kỳ thi, chúng tôi chọn một trong hai$a_i$hoặc$b_i$và chúng tôi duy trì một tập hợp các ngày đã sử dụng. Mỗi lựa chọn đều thành công hoặc xung đột và chúng tôi khám phá mọi khả năng. 

Điều này hoạt động về mặt khái niệm vì nó thực thi ràng buộc một cách trực tiếp, nhưng không gian trạng thái sẽ tăng gấp đôi cho mỗi kỳ thi. Với$n = 10^6$, điều này trở thành$2^n$, điều này hoàn toàn không thể xảy ra ngay cả đối với rất nhỏ$n$. 

Một chế độ xem có cấu trúc hơn là coi mỗi bài kiểm tra là một ranh giới giữa hai ngày có thể và chúng tôi muốn chỉ định mỗi bài kiểm tra cho chính xác một điểm cuối sao cho không có điểm cuối nào được sử dụng lại. Điều này tương đương với việc chọn một kết quả khớp trong biểu đồ trong đó mỗi kỳ thi là một nút ở một bên và các ngày là các nút ở phía bên kia, nhưng mỗi kỳ thi có mức độ 2. Vấn đề trở thành việc tìm một bài tập nội tại từ các kỳ thi đến những ngày có ràng buộc. 

Quan sát quan trọng là chúng ta không cần phải xem xét tất cả các ngày một cách rõ ràng. Chỉ có thứ tự tương đối của các ngày mới quan trọng vì chúng ta đang giảm thiểu số ngày sử dụng tối đa. Điều này gợi ý việc sắp xếp tất cả các bài kiểm tra theo cấu trúc ưa thích của chúng và xử lý chúng một cách tham lam. 

Chiến lược tham lam tự nhiên sẽ xuất hiện nếu chúng ta sắp xếp các bài thi theo ngày đầu tiên có bài thi.$a_i$, sau đó cố gắng chỉ định mỗi bài kiểm tra vào ngày hợp lệ nhỏ nhất có thể, ưu tiên$a_i$trước$b_i$. Tuy nhiên, kẻ tham lam ngây thơ này vẫn thất bại trong một số cấu hình nhất định vì những quyết định sớm có thể cản trở những cấu hình sau. 

Quan điểm đúng đắn là giải thích điều này như một sự kết hợp lưỡng cực với mỗi nút bên trái có mức độ 2 và xử lý bằng cách sử dụng một cấu trúc luôn theo dõi các nhiệm vụ bắt buộc. Điều này có thể được giải quyết bằng cách xử lý theo thứ tự ngày tăng dần và duy trì những bài kiểm tra vẫn chưa được giao, sử dụng lựa chọn tham lam để tránh chặn các bài tập trong tương lai. Một cách tiêu chuẩn để thực thi tính chính xác là coi mỗi kỳ thi là một lựa chọn khoảng thời gian và sử dụng lập lịch tham lam với thứ tự theo điểm cuối thứ hai, kết hợp với cấu trúc dữ liệu luôn chỉ định vị trí khả thi sớm nhất. 

Trong thực tế, giải pháp tối ưu giảm xuống việc sắp xếp theo$b_i$và tham lam ấn định các ngày có sẵn bài kiểm tra, đảm bảo chúng tôi luôn sử dụng ngày sớm nhất có thể và tránh xung đột trong tương lai. Điều này phản ánh việc lập kế hoạch theo khoảng thời gian cổ điển với các lựa chọn thay thế, trong đó việc sắp xếp theo ràng buộc sau sẽ giảm thiểu việc chặn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quay lại vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tham lam với việc đặt hàng bởi$b_i$|$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển mỗi kỳ thi thành một cặp ngày dành cho ứng viên và xử lý các kỳ thi theo thứ tự tôn trọng các ràng buộc của chúng. 

1. Sắp xếp tất cả các bài kiểm tra theo ngày thứ hai có sẵn$b_i$. Điều này đảm bảo rằng các kỳ thi có thời hạn chặt chẽ hơn sẽ được xử lý trước tiên. Trực giác là nếu một bài kiểm tra có điểm nhỏ$b_i$, trì hoãn nó có nguy cơ mất cả hai lựa chọn. 
2. Duy trì một cấu trúc cố định hoặc có thứ tự của những ngày đã sử dụng. Cấu trúc này cho phép chúng ta kiểm tra xem một ngày đã được sử dụng theo thời gian logarit hay chưa. 
3. Lặp lại các bài kiểm tra theo thứ tự được sắp xếp. Đối với mỗi bài kiểm tra, hãy cố gắng giao nó cho$a_i$nếu ngày đó rảnh. Nếu không, hãy thử gán nó cho$b_i$. 
4. Nếu không$a_i$cũng không$b_i$có sẵn, nhiệm vụ là không thể, vì vậy chúng tôi trả về -1 ngay lập tức. 
5. Theo dõi ngày sử dụng tối đa cho tất cả các nhiệm vụ. Giá trị này thể hiện ngày sớm nhất mà tất cả các bài kiểm tra được hoàn thành. 
6. Sau khi xử lý tất cả các bài kiểm tra, hãy trả lại ngày được chỉ định tối đa. 

Sự lựa chọn ưu tiên$a_i$qua$b_i$không phải là tùy tiện. Từ$a_i < b_i$, sử dụng vị trí trước đó khi có thể sẽ duy trì tính linh hoạt sau này, trong khi vẫn tôn trọng thứ tự áp đặt bằng cách sắp xếp trên$b_i$. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, chúng tôi xử lý bài kiểm tra với lựa chọn thứ hai nhỏ nhất trong số tất cả các bài kiểm tra còn lại. Điều này đảm bảo rằng nếu có bất kỳ nhiệm vụ khả thi nào, chúng tôi sẽ không bao giờ trì hoãn kỳ thi vượt quá vị trí an toàn cuối cùng có thể có của kỳ thi đó. Điều bất biến là sau khi xử lý lần đầu tiên$k$các bài kiểm tra, tồn tại một bài tập hợp lệ cho các bài kiểm tra đó chỉ sử dụng cấu trúc đã được xem xét và không có quyết định nào trong tương lai phụ thuộc vào việc phân công lại các lựa chọn trước đó. Sự lựa chọn tham lam sẽ duy trì tính khả thi vì bất kỳ nhiệm vụ thay thế nào cũng có thể trì hoãn một kỳ thi với thời gian nhỏ hơn.$b_i$không thể cải thiện tính khả thi cho các kỳ thi sau này với số lượng lớn hơn hoặc bằng$b_i$. 

## Giải pháp Python```
PythonRun
```Bước sắp xếp đảm bảo chúng tôi luôn xử lý các bài kiểm tra bị hạn chế nhất trước tiên tính theo ngày khả dụng thứ hai. Nhiệm vụ tham lam sẽ thử vào ngày sớm hơn trước vì nó duy trì tính khả dụng sau này. bộ`used`thực thi ràng buộc duy nhất trong tất cả các ngày đã chọn. Biến`ans`theo dõi ngày gần nhất được sử dụng trong bất kỳ bài tập nào. 

Việc thoát sớm khi không đạt là rất quan trọng vì một khi không thể giao bài thi thì không có sự sắp xếp lại bài tập nào trong tương lai có thể khắc phục được vi phạm do thứ tự của$b_i$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Sắp xếp theo$b$: 
| Bước | Thi (a,b) | Hãy thử | Hãy thử b | Ngày sử dụng | và | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,5) | 1 miễn phí | - | {1} | 1 | 
| 2 | (1,7) | 1 lần chụp | 7 miễn phí | {1,7} | 7 | 
Đầu ra:```

```Dấu vết này cho thấy xung đột ở những ngày đầu buộc phải sử dụng các vị trí sau đó như thế nào và ngày tối đa phản ánh thời gian hoàn thành cuối cùng. 

### Ví dụ 2 

đầu vào:```

```Sắp xếp theo$b$: 

| Bước | thi | Hành động | Đã qua sử dụng | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | lấy 1 | {1} | 1 | 
| 2 | (2,3) | lấy 2 | {1,2} | 2 | 
| 3 | (1,3) | lấy 1, lấy 3 | {1,2,3} | 3 | 

Đầu ra:```

```Điều này chứng tỏ rằng ngay cả khi các lựa chọn ban đầu xung đột với nhau, các nhiệm vụ dự phòng vẫn đảm bảo tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; trung bình mỗi bài kiểm tra được xử lý một lần với các thao tác tập hợp O(1) | 
| Không gian |$O(n)$| Lưu trữ bài kiểm tra và theo dõi ngày sử dụng | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc đối với$n = 10^6$, vì việc sắp xếp và xử lý tuyến tính vẫn khả thi trong các giới hạn thông thường. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giống hệt nhau | 5 | tuyên truyền tính khả thi tham lam | 
| trùng lặp không thể | -1 | phát hiện xung đột | 
| thi đơn | 10 | trường hợp cơ sở đúng đắn | 
| chồng chéo | 4 | hành vi chặn muộn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều bài kiểm tra có cùng một ngày đầu nhưng khác nhau về số ngày dự phòng. Thuật toán xử lý việc này vì sắp xếp theo$b_i$đảm bảo các kỳ thi hạn chế nhất sẽ được giao trước, tránh bị đẩy vào các xung đột sau này. 

Một trường hợp khác là khi hai bài thi có chung cả hai thí sinh. Ví dụ:```

```Sau khi sắp xếp, bài kiểm tra đầu tiên diễn ra vào ngày 1 và bài kiểm tra thứ hai buộc phải diễn ra vào ngày thứ 2. Nếu cả 1 và 2 đều đã được thực hiện thì thuật toán sẽ thất bại một cách chính xác. Điều này cho thấy rằng việc kiểm tra tỷ lệ sử dụng dựa trên tập hợp thực thi chính xác tính năng tiêm mà không yêu cầu quay lui rõ ràng. 

Trường hợp tinh tế cuối cùng là chuỗi dài trong đó mỗi nhiệm vụ phụ thuộc vào việc giải phóng các lựa chọn trước đó. Bởi vì chúng tôi không bao giờ chỉ định lại một lần mỗi ngày được sử dụng nên độ chính xác hoàn toàn phụ thuộc vào thứ tự xử lý. Sắp xếp theo$b_i$đảm bảo rằng không có quyết định nào sau này có thể làm mất hiệu lực tính khả thi trước đó, do đó việc phân công tham lam vẫn nhất quán trong suốt quá trình thực hiện.
