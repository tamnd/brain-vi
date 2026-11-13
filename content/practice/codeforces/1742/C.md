---
title: "CF 1742C - Sọc"
description: "Nhiệm vụ trình bày một lưới 8 × 8 trong đó một số hàng được sơn màu đỏ và một số cột được sơn màu xanh lam. Việc vẽ diễn ra tuần tự: khi một sọc được vẽ, nó sẽ ghi đè lên tất cả các ô dọc theo hàng hoặc cột đó, ngay cả khi chúng đã có màu."
date: "2026-06-09T16:16:34+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1742
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 827 (Div. 4)"
rating: 900
weight: 1742
solve_time_s: 341
verified: true
draft: false
---

[CF 1742C - Sọc](https://codeforces.com/problemset/problem/1742/C) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 5 phút 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ trình bày một lưới 8 × 8 trong đó một số hàng được sơn màu đỏ và một số cột được sơn màu xanh lam. Việc vẽ diễn ra tuần tự: khi một sọc được vẽ, nó sẽ ghi đè lên tất cả các ô dọc theo hàng hoặc cột đó, ngay cả khi chúng đã có màu. Lưới chúng tôi nhận được dưới dạng đầu vào hiển thị các màu cuối cùng, bao gồm các ô không được tô màu được biểu thị bằng dấu chấm. Mục đích là để xác định màu nào, đỏ hay xanh, được sơn cuối cùng. Điều đó tương đương với việc hỏi loại sọc nào, hàng hoàn toàn màu đỏ hoặc cột hoàn toàn màu xanh lam, chiếm ưu thế trong lưới cuối cùng sao cho nó phải được sơn cuối cùng. 

Mỗi test case cung cấp chính xác 8 dòng 8 ký tự và có thể có tới 4000 test case. Vì lưới rất nhỏ nên mọi phép toán tuyến tính hoặc bậc hai về số lượng ô đều có thể chấp nhận được. Việc cân nhắc chính là diễn giải chính xác màu cuối cùng theo trình tự vẽ chứ không phải bản thân quá trình vẽ. Các trường hợp cạnh bao gồm các tình huống tồn tại nhiều sọc của mỗi màu, nhưng chỉ có một màu có thể là cuối cùng. Ví dụ: nếu hàng dưới cùng hoàn toàn màu đỏ và tất cả các cột có một phần màu xanh lam thì sọc cuối cùng phải có màu đỏ, vì hàng màu đỏ cuối cùng chỉ có thể được sơn cuối cùng để trông nguyên vẹn. Ngược lại, nếu một cột hoàn toàn có màu xanh lam và tất cả các hàng có một phần màu đỏ thì sọc cuối cùng phải có màu xanh lam. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng xây dựng lại trình tự vẽ tranh bằng cách kiểm tra mọi thứ tự có thể có của các hàng và cột. Người ta có thể liệt kê tất cả các chuỗi có thể có của các ứng dụng sọc và xem kết quả nào trong lưới cuối cùng được quan sát. Điều này hoạt động về nguyên tắc nhưng phức tạp không cần thiết. Số lượng thứ tự sọc có thể tăng theo cấp số nhân với số lượng sọc được sơn, khiến cho lực lượng mạnh mẽ như vậy trở nên không thực tế ngay cả đối với lưới nhỏ này nếu cố gắng bất cẩn. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng sọc cuối cùng xác định đường kẻ được tô đầy đủ màu của nó trong lưới cuối cùng. Hàng màu đỏ được sơn cuối cùng khiến toàn bộ hàng có màu đỏ bất kể các cột màu xanh trước đó. Tương tự, cột màu xanh lam được sơn cuối cùng sẽ khiến toàn bộ cột có màu xanh lam. Do đó, để xác định màu nào được sơn cuối cùng, chỉ cần kiểm tra bất kỳ hàng nào hoàn toàn màu đỏ hoặc bất kỳ cột nào hoàn toàn màu xanh lam. Nếu có hàng màu đỏ toàn bộ thì sọc cuối cùng phải có màu đỏ; nếu có cột toàn màu xanh lam thì sọc cuối cùng phải có màu xanh lam. Vấn đề đảm bảo rằng lưới được tạo ra bởi các sọc như vậy, do đó luôn có ít nhất một sọc được sơn cuối cùng có thể được phát hiện theo cách này. 

Quan sát này làm giảm vấn đề từ việc cố gắng tái tạo lại các chuỗi sang việc quét các hàng và cột đơn giản. Chúng tôi lặp lại qua tất cả các hàng để xem có hàng nào hoàn toàn màu đỏ không và qua tất cả các cột để xem có hàng nào hoàn toàn màu xanh lam hay không. Vì lưới được cố định ở mức 8×8 nên quá trình quét này thực hiện tối đa 64 thao tác cho mỗi trường hợp kiểm thử, con số này không đáng kể ngay cả đối với 4000 trường hợp kiểm thử. Thông tin chi tiết tận dụng cấu trúc của vấn đề: các sọc lấp đầy hàng hoặc cột của chúng và ghi đè các màu trước đó, làm cho sọc cuối cùng có thể xác định được từ mức độ hoàn chỉnh trong lưới cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tái thiết trình tự vũ phu | O(2^16) | O(64) | Quá chậm | 
| Quét toàn bộ hàng/cột | O(64 cho mỗi trường hợp thử nghiệm) | O(64) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số lượng test case. Đối với mỗi trường hợp kiểm thử, hãy đọc lưới 8×8 dưới dạng danh sách các chuỗi. 
2. Lặp lại qua từng hàng trong số 8 hàng. Đối với một hàng, hãy kiểm tra xem tất cả 8 ô có phải là 'R' hay không. Nếu tìm thấy một hàng như vậy, hãy xuất 'R' cho trường hợp kiểm thử này và tiếp tục tới trường hợp kiểm thử tiếp theo. Bước này có hiệu quả vì hàng hoàn toàn màu đỏ chỉ có thể xuất hiện nếu sọc đỏ được sơn cuối cùng trên hàng đó, đảm bảo tất cả các ô đều có màu đỏ. 
3. Nếu không tìm thấy hàng toàn màu đỏ, hãy lặp qua 8 cột. Đối với một cột, hãy kiểm tra xem tất cả 8 ô có phải là 'B' hay không. Nếu tìm thấy cột như vậy, hãy xuất 'B'. Sự tồn tại của một cột hoàn toàn màu xanh ngụ ý rằng sọc xanh lam được sơn cuối cùng trên cột đó, làm cho sọc cuối cùng có màu xanh lam. 
4. Tiếp tục quá trình này cho tất cả các trường hợp thử nghiệm. Vì vấn đề đảm bảo tồn tại ít nhất một sọc nên một trong hai điều kiện này sẽ luôn được đáp ứng, do đó không cần xử lý bổ sung cho các lưới trống. 

Thuật toán này hoạt động vì sọc cuối cùng của một màu nhất định là sọc duy nhất có thể để lại toàn bộ hàng hoặc cột hoàn toàn bằng màu đó. Bất kỳ sọc nào được sơn trước đó sẽ bị ghi đè một phần bởi sọc cuối cùng. Do đó, việc quét hàng toàn màu đỏ hoặc cột toàn màu xanh là đủ để xác định sọc cuối cùng một cách rõ ràng. 

## Giải pháp Python```
PythonRun
```Trước tiên, giải pháp sẽ sử dụng số lượng trường hợp thử nghiệm và xử lý dòng trống trước mỗi lưới. Việc kiểm tra hàng màu đỏ hoàn toàn được thực hiện trước khi kiểm tra cột vì sự cố đảm bảo rằng có ít nhất một sọc tồn tại và chỉ có một màu có thể là cuối cùng. Lặp lại qua các hàng và cột sử dụng Python`all`chức năng để xác minh tính đầy đủ, ngắn gọn và tránh các lỗi sai sót. Thứ tự của các bước kiểm tra này phù hợp với việc đảm bảo rằng có chính xác một sọc cuối cùng. Việc loại bỏ đầu vào đảm bảo không có dòng mới ở cuối ảnh hưởng đến việc kiểm tra hàng. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên, lưới có hàng màu đỏ hoàn toàn ở chỉ số 3 (dựa trên 0). Lặp lại qua các hàng sẽ tìm thấy hàng đó và ngay lập tức trả về 'R'. Không cần kiểm tra cột. Đối với đầu vào mẫu thứ hai, không có hàng màu đỏ hoàn toàn, nhưng cột 0 và cột 7 có màu xanh lam hoàn toàn. Kiểm tra hàng không thành công, sau đó kiểm tra cột sẽ tìm thấy cột màu xanh lam và xuất ra 'B'. 

| Bước | Lưới/Hàng/Cột | Kiểm tra màu lần cuối | Đầu ra | 
| --- | --- | --- | --- | 
| Trường hợp thử nghiệm 1 | hàng 0-7 | hàng 3 tất cả 'R' | R | 
| Trường hợp thử nghiệm 2 | hàng 0-7 | không có hàng tất cả 'R' | kiểm tra các cột: col 0 tất cả 'B' | 

Những dấu vết này cho thấy thuật toán xác định chính xác sọc cuối cùng dựa trên các hàng hoặc cột hoàn chỉnh, ngay cả khi có nhiều sọc của mỗi màu tồn tại trong lưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * 64) | Đối với mỗi trường hợp thử nghiệm, chúng tôi quét tất cả 8 hàng và 8 cột. Với t 4000, tổng số thao tác 256.000 | 
| Không gian | O(64) | Mỗi trường hợp thử nghiệm lưu trữ lưới 8 × 8 | 

Độ phức tạp về thời gian nằm dưới giới hạn 1 giây một cách thoải mái. Việc sử dụng không gian là tối thiểu vì chỉ cần giữ lưới hiện tại trong bộ nhớ. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tùy chỉnh đầu tiên | B, R | Xác định sọc cuối cùng ở hàng trên cùng so với cột bên trái | 
| Tùy chỉnh thứ hai | B | Sọc cuối cùng là hàng duy nhất có màu xanh lam hoàn toàn ở dưới cùng | 
| Tùy chỉnh thứ ba | R | Toàn bộ lưới |
