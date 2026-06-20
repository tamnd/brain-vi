---
title: "CF 1037D - BFS hợp lệ?"
description: "Chúng ta được cho một cây có các đỉnh được đánh số từ 1 đến n và thứ tự đề xuất của tất cả các đỉnh. Nhiệm vụ là quyết định xem thứ tự này có thể phát sinh từ việc thực hiện tìm kiếm theo chiều rộng bắt đầu từ đỉnh 1 hay không, dưới một số lựa chọn hợp lệ về thứ tự kề."
date: "2026-06-16T18:47:21+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1037
codeforces_index: "D"
codeforces_contest_name: "Manthan, Codefest 18 (rated, Div. 1 + Div. 2)"
rating: 1700
weight: 1037
solve_time_s: 293
verified: false
draft: false
---

[CF 1037D - BFS hợp lệ?](https://codeforces.com/problemset/problem/1037/D) 

**Đánh giá:** 1700 
**Thẻ:** dfs và tương tự, đồ thị, đường đi ngắn nhất, cây 
**Thời gian giải:** 4m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các đỉnh được đánh số từ 1 đến n và thứ tự đề xuất của tất cả các đỉnh. Nhiệm vụ là quyết định xem thứ tự này có thể phát sinh từ việc thực hiện tìm kiếm theo chiều rộng bắt đầu từ đỉnh 1 hay không, dưới một số lựa chọn hợp lệ về thứ tự kề. 

Điểm tinh tế quan trọng là BFS không mang tính quyết định trên cây vì khi chúng tôi mở rộng một nút, chúng tôi có thể tự do thăm các nút lân cận chưa được thăm dò của nó theo bất kỳ thứ tự nào. Các thứ tự thăm dò lân cận khác nhau có thể tạo ra các chuỗi BFS toàn cầu khác nhau. Câu hỏi đặt ra không phải là liệu chuỗi đó có phải là BFS nhỏ nhất về mặt từ điển hay BFS có tính kề cận được sắp xếp hay không, mà là liệu có tồn tại bất kỳ thứ tự lân cận nào tạo ra chính xác chuỗi đã cho hay không. 

Ràng buộc n lên tới 200.000 ngụ ý rằng mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính. Cách tiếp cận O(n log n) có thể chấp nhận được, nhưng bất cứ điều gì liên tục sắp xếp hoặc mô phỏng BFS với cấu trúc dữ liệu đắt tiền trên mỗi nút đều phải được thiết kế cẩn thận để tránh hành vi bậc hai. Cấu trúc cây đảm bảo n trừ 1 cạnh, do đó danh sách kề rất thưa thớt và chúng ta có thể dựa vào các kỹ thuật duyệt đồ thị tiêu chuẩn. 

Một ý tưởng ngây thơ nhưng hấp dẫn là mô phỏng BFS trong khi cố gắng khớp chuỗi đã cho một cách tham lam. Điều này không thành công vì BFS cho phép mở rộng biên giới hợp lệ nhiều lần và các quyết định cục bộ sai về thứ tự kề có thể dẫn đến việc từ chối không chính xác. 

Một vài trường hợp đặc biệt quan trọng minh họa những cạm bẫy. Đầu tiên, nếu chuỗi không bắt đầu bằng 1 thì ngay lập tức nó không hợp lệ vì BFS luôn bắt đầu ở đó. Ví dụ: nhập theo trình tự`2 1 3`trên cây hợp lệ phải xuất ra "Không". Thứ hai, ngay cả khi trình tự bắt đầu chính xác, một bước hợp lệ cục bộ vẫn có thể phá vỡ tính hợp lệ toàn cục. Ví dụ: nếu hàng đợi chứa các nút`{2, 3}`nhưng trình tự dự kiến ​​​​sẽ truy cập vào phần tử con sâu của 2 trước khi kết thúc tất cả các hàng xóm của 1, điều đó là không thể trong BFS bất kể thứ tự kề. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ thử tất cả các thứ tự kề cận có thể có cho mỗi nút, chạy BFS cho mỗi thứ tự và kiểm tra xem kết quả truyền tải có khớp với chuỗi mục tiêu hay không. Vì mỗi nút bậc d có d! hoán vị, điều này nhanh chóng bùng nổ. Ngay cả trong một cây, mức độ trong trường hợp xấu nhất có thể lớn, vì vậy đây là cấp số nhân trong thực tế và hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là BFS không phụ thuộc vào thứ tự chính xác mà chúng tôi xếp hàng các hàng xóm cục bộ, mà phụ thuộc vào ràng buộc toàn cầu rằng các nút được truy cập theo lớp. Nếu chúng tôi sửa thứ tự BFS mục tiêu thì các nút sẽ được nhóm bằng cách tăng khoảng cách từ 1 và trong cùng một lớp khoảng cách, thứ tự tương đối phải nhất quán với việc mở rộng biên giới BFS. 

Chúng ta có thể đảo ngược quan điểm: thay vì mô phỏng BFS một cách tự do, chúng ta mô phỏng nó đồng thời buộc hàng đợi xử lý các đỉnh theo thứ tự chính xác do trình tự đưa ra. Chúng tôi sử dụng hàng đợi và so sánh nó với trình tự, nhưng thủ thuật quan trọng là khi mở rộng một nút, chúng tôi thu thập tất cả các nút lân cận chưa được truy cập của nó và gán cho chúng một thứ tự do vị trí của chúng tạo ra trong chuỗi mục tiêu. Nếu chuỗi là BFS hợp lệ thì đối với mỗi nút, các nút con của nó trong cây BFS phải xuất hiện dưới dạng khối liền kề trong chuỗi ngay lập tức.
