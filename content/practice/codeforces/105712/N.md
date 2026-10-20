---
title: "CF 105712N - Tách chuỗi"
description: "Chúng ta được cấp một chuỗi bắt đầu s và một chuỗi đích t. Thao tác duy nhất được phép trên s là loại bỏ tất cả các ký tự hiện đang chiếm vị trí lẻ hoặc tất cả các ký tự hiện chiếm vị trí chẵn."
date: "2026-06-26T08:53:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105712
codeforces_index: "N"
codeforces_contest_name: "Rutgers University Programming Contest Fall 2024"
rating: 0
weight: 105712
solve_time_s: 28
verified: false
draft: false
---

[CF 105712N - Tách chuỗi](https://codeforces.com/problemset/problem/105712/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi bắt đầu`s`và một chuỗi mục tiêu`t`. Hoạt động duy nhất được phép trên`s`là loại bỏ tất cả các ký tự hiện chiếm vị trí lẻ hoặc tất cả các ký tự hiện chiếm vị trí chẵn. Thao tác có thể được lặp lại bao nhiêu lần tùy ý và câu hỏi đặt ra là liệu có tồn tại chuỗi thao tác nào đó thay đổi hay không?`s`vào chính xác`t`. 

Khó khăn chính là các vị trí đều dựa trên chuỗi hiện tại sau mỗi lần xóa. Một nhân vật ban đầu ở một vị trí có thể di chuyển đến một vị trí khác sau đó, do đó việc mô phỏng các lựa chọn xóa ngẫu nhiên có thể nhanh chóng gây hiểu lầm. Giải pháp xuất phát từ việc hiểu được hình dạng của một chuỗi sau khi phân tách nhiều lần. 

Tổng chiều dài của tất cả các chuỗi đầu vào tối đa là`2 * 10^5`. Điều này có nghĩa là thuật toán phải gần tuyến tính. Một cách tiếp cận bậc hai so sánh mọi chuỗi xóa có thể hoặc mọi chuỗi có thể xảy ra sẽ thực hiện xung quanh`4 * 10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn một giây thông thường cho phép. 

Các trường hợp cạnh quan trọng xuất phát từ sự thay đổi độ dài và từ thực tế là một thao tác xóa đơn lẻ không cho phép các chuỗi con tùy ý. Ví dụ: nếu đầu vào là:```
abcdefg
cde
```câu trả lời là:```
NO
```Một giải pháp bất cẩn có thể nghĩ rằng bất kỳ chuỗi con hoặc chuỗi con nào cũng có thể được tạo ra, nhưng sau một thao tác, các chuỗi có thể chỉ được tạo ra.`aceg`hoặc`bdf`. các nhân vật`cde`không bao giờ xuất hiện cùng nhau. 

Một trường hợp cạnh khác là khi mục tiêu đã bằng chuỗi gốc:```
baccba
baccba
```Đầu ra đúng là:```
YES
```bởi vì không có hoạt động nào được phép. Việc triển khai luôn thực hiện ít nhất một thao tác xóa sẽ từ chối trường hợp này một cách không chính xác. 

Trường hợp khó khăn cuối cùng là khi mục tiêu dài hơn nguồn:```
nick
james
```Đầu ra là:```
NO
```Mọi thao tác chỉ giữ lại một phần của chuỗi nên độ dài không bao giờ có thể tăng lên. Bất kỳ giải pháp nào chỉ kiểm tra mối quan hệ ký tự mà quên hạn chế này đều có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tạo ra mọi chuỗi có thể truy cập được từ`s`. Vì mọi thao tác đều chọn một trong hai nửa vị trí hiện tại nên việc tìm kiếm theo chiều rộng trên các trạng thái cuối cùng sẽ tìm ra liệu`t`xuất hiện. Điều này đúng vì nó khám phá chính xác những biến đổi có thể xảy ra. 

Vấn đề là số lượng các bang tăng lên quá nhanh. Một chuỗi có độ dài`n`có thể liên tục chia thành các chuỗi nhỏ hơn và thử cả hai lựa chọn ở mọi cấp độ sẽ tạo ra cây tìm kiếm theo cấp số nhân. Mặc dù nhiều nhánh có thể hợp nhất, việc lưu trữ và so sánh tất cả các chuỗi được tạo ra là quá tốn kém đối với độ dài gần`2 * 10^5`. 

Quan sát loại bỏ nhu cầu mô phỏng là xem xét cấu trúc của tất cả các chuỗi có thể truy cập được. Mỗi thao tác giữ các ký tự có một chỉ số chẵn lẻ. Nếu chúng ta xem xét các chỉ số ban đầu, sau một thao tác, chúng ta sẽ giữ tất cả các chỉ số có cùng tính chẵn lẻ. Sau một thao tác khác, chúng tôi lại lấy mọi ký tự khác của chuỗi đã được lọc đó. Điều này có nghĩa là các ký tự còn lại luôn tương ứng với các vị trí tuân theo mẫu thông thường trong chuỗi gốc. 

Chính xác hơn, mọi chuỗi có thể truy cập đều có được bằng cách chọn một số vị trí bắt đầu và liên tục lấy các ký tự có kích thước bước cố định là lũy thừa của hai. Tuy nhiên, do mỗi thao tác chia đôi chuỗi hiện tại nên các khả năng duy nhất tồn tại cho đến độ dài mục tiêu được xác định bằng cách liên tục chọn một trong hai nhóm chẵn lẻ. 

Hậu quả quan trọng là nếu độ dài mục tiêu là`m`, thì thao tác đầu tiên khả thi duy nhất là thao tác tạo ra một chuỗi có độ dài ít nhất`m`và sau khi tiếp tục quá trình, mục tiêu phải xuất hiện dưới dạng chuỗi con cuối cùng được tạo bởi các lựa chọn chẵn lẻ này. Điều này có thể được kiểm tra trực tiếp hơn nhiều bằng cách quan sát bản chất nhị phân của sự phân chia. 

Một cách hữu ích để xem quá trình là từ mục tiêu trở lại. Nếu một chuỗi`x`có thể trở thành`y`sau một lần thao tác thì`y`phải là các ký tự của`x`tại các vị trí`0,2,4,...`hoặc các ký tự ở vị trí`1,3,5,...`. Đảo ngược mối quan hệ này có nghĩa là mọi chuỗi chuyển đổi hợp lệ đều tương ứng với việc chọn liên tục một nhóm chẵn lẻ cho đến khi độ dài còn lại trở thành chính xác độ dài mục tiêu. 

Vì mỗi lần phân chia làm giảm độ dài khoảng một nửa nên số lượng độ sâu có thể có là nhỏ. Thuật toán cuối cùng chỉ cần tuân theo các lựa chọn chẵn lẻ có thể có thay vì khám phá tất cả các chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong` | s | `| 
| Tối ưu | O(` | s | + | 

## Hướng dẫn thuật toán 

1. Nếu chuỗi đích dài hơn chuỗi gốc, hãy trả lời ngay`NO`. Mọi thao tác đều loại bỏ các ký tự nên việc tăng độ dài là không thể. 
2. Kiểm tra xem có thể lấy được mục tiêu hay không bằng cách liên tục lấy từng ký tự thứ hai từ chuỗi hiện tại. Thay vì mô phỏng tất cả các nhánh có thể, hãy xem xét đệ quy hai khả năng con của chuỗi hiện tại: các ký tự từ các chỉ số chẵn và các ký tự từ các chỉ số lẻ. 
3. Dừng đệ quy khi độ dài chuỗi hiện tại bằng độ dài mục tiêu. Tại thời điểm đó, kết quả hợp lệ duy nhất là kiểm tra tính bằng nhau của chuỗi chính xác. 
4. Ghi nhớ các chuỗi đã được kiểm tra. Có thể đạt được cùng một chuỗi trung gian thông qua các chuỗi thao tác khác nhau và việc lưu trữ các trạng thái lỗi sẽ ngăn cản công việc lặp lại. 
5. Đầu ra`YES`nếu bất kỳ chuỗi lựa chọn chẵn lẻ nào đạt được mục tiêu, nếu không thì xuất ra`NO`. 

Lý do điều này có tác dụng là vì mọi hoạt động hợp pháp đều có chính xác hai kết quả: giữ một vị thế ngang bằng với nhau. Phép đệ quy kiểm tra cả hai kết quả đó, vì vậy nó không thể bỏ lỡ một chuỗi hợp lệ. Việc ghi nhớ chỉ loại bỏ việc kiểm tra lặp đi lặp lại các trạng thái giống hệt nhau và không thay đổi tập hợp các chuỗi có thể truy cập đang được xem xét.
