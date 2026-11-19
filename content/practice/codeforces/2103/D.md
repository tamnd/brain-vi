---
title: "CF 2103D - Xây dựng địa phương"
description: "Đối với mỗi vị trí trong một hoán vị không xác định, chúng ta được cung cấp phép lặp mà tại đó phần tử ở vị trí đó biến mất trong quá trình cắt tỉa xác định."
date: "2026-06-08T05:02:33+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2103
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1019 (Div. 2)"
rating: 2000
weight: 2103
solve_time_s: 41
verified: false
draft: false
---

[CF 2103D - Xây dựng địa phương](https://codeforces.com/problemset/problem/2103/D) 

**Xếp hạng:** 2000 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, triển khai, hai con trỏ 
**Thời gian giải:** 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi vị trí trong một hoán vị không xác định, chúng ta được cung cấp phép lặp mà tại đó phần tử ở vị trí đó biến mất trong quá trình cắt tỉa xác định. Hoán vị được lọc nhiều lần: trong các lần lặp lẻ, chúng tôi chỉ giữ lại cực tiểu cục bộ và trên các lần lặp chẵn, chúng tôi chỉ giữ lại cực đại cục bộ, cho đến khi còn lại một phần tử. Do đó, mỗi nguyên tố đều có một “thời điểm chết” hoặc được đánh dấu là tồn tại mãi mãi. 

Nhiệm vụ là tái tạo lại bất kỳ hoán vị nào tạo ra chính xác những thời gian loại bỏ này theo quy trình loại bỏ cực trị cục bộ xen kẽ đó. 

Khó khăn là quá trình này mang tính phi cục bộ cao. Việc một phần tử có tồn tại qua vòng lặp hay không phụ thuộc vào sự so sánh với các phần tử lân cận và những phần tử lân cận đó sẽ thay đổi sau mỗi bước cắt tỉa. Vì vậy, các ràng buộc về thời gian loại bỏ mã hóa cấu trúc phân lớp trên hoán vị, không phải các ràng buộc độc lập cho mỗi vị trí. 

Các ràng buộc rất lớn: tổng n trên tất cả các trường hợp thử nghiệm lên tới 2⋅10^5. Bất kỳ giải pháp nào mô phỏng trực tiếp quá trình đều quá chậm, vì mỗi lần lặp sẽ quét toàn bộ mảng và có tới log n lần lặp, tạo ra O(n log n) là đường biên nhưng vẫn phức tạp do tái cấu trúc. Quan trọng hơn, mô phỏng trực tiếp không phải là vấn đề cốt lõi mà là tái thiết. 

Trường hợp cạnh tinh tế quan trọng xuất hiện khi nhiều phần tử có cùng thời gian loại bỏ. Ví dụ: nếu tất cả a_i = 1 ngoại trừ một phần tử thì hoán vị cuối cùng phải buộc chính xác một mức tối thiểu cục bộ ở bước 1. Vị trí tham lam ngây thơ thường thất bại vì nó không thực thi các ràng buộc thứ tự nhất quán trên nhiều lớp loại bỏ. 

Một trường hợp cạnh khác là khi một số phần tử tồn tại đến cuối (a_i = -1). Chúng phải tạo thành phần sống sót duy nhất thông qua việc giảm thiểu tối thiểu và tối đa xen kẽ, nghĩa là chúng là gốc của cấu trúc đệ quy. Bất kỳ công trình xây dựng nào không bảo tồn rõ ràng một “chuỗi sinh tồn” nhất quán duy nhất đều có xu hướng bị phá vỡ ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử xây dựng hoán vị và mô phỏng quá trình cắt tỉa trong khi điều chỉnh hoán vị cho đến khi thời gian loại bỏ được ghi phù hợp. Điều này nhanh chóng trở nên không khả thi vì mỗi điều chỉnh sẽ thay đổi nhiều mối quan hệ cục bộ và việc xác minh tính chính xác đòi hỏi phải mô phỏng lại tới n lớp nhật ký. Ngay cả một cách tiếp cận quay lui cẩn thận cũng có thể làm giảm hành vi theo cấp số nhân trong các trường hợp đối nghịch. 

Cái nhìn sâu sắc quan trọng là đảo ngược quá trình. 

Thay vì nghĩ xem ai sống sót sau mỗi lần lặp, chúng tôi nghĩ về cách phần tử cuối cùng còn lại phải thống trị một cấu trúc được phân hủy đệ quy thành các “lớp thung lũng” và “lớp đỉnh” xen kẽ. Mỗi lần lặp lại sẽ chia phân đoạn hiện tại thành những phân đoạn sống sót xen kẽ giữa cực tiểu và cực đại. Điều này có cấu trúc tương đương với việc gán các phần tử cho các cấp trong một phân rã dạng cây trong đó mỗi cấp tương ứng với một lần lặp. 

Chúng ta có thể gán các giá trị không phải bằng cách mô phỏng hoán vị mà bằng cách xây dựng nó theo từng lớp: trước tiên chỉ định phần tử sống sót cuối cùng, sau đó gán đệ quy các phần tử nào phải là cực tiểu hoặc cực đại cục bộ ở mỗi cấp độ và cuối cùng chuyển hệ thống phân cấp này thành hoán vị bằng cách sử dụng nhãn tăng dần. 

Quan sát quan trọng là quy trình xác định sự phân chia các chỉ số thành các nhóm theo thời gian loại bỏ và các nhóm này phải luân phiên giữa các “lớp thung lũng” và “các lớp đỉnh”. Trong mỗi lớp, các phần tử tồn tại lâu hơn phải tạo thành cấu trúc liền kề trong hoán vị được xây dựng để chúng có thể trở thành cực trị cục bộ vào đúng thời điểm.

Điều này dẫn đến một cấu trúc dựa trên việc sắp xếp các chỉ mục theo DFS tùy chỉnh tôn trọng cấu trúc xen kẽ của các lớp: chúng tôi chia đệ quy các phân đoạn thành các phần bên trái và bên phải tùy thuộc vào việc lớp hiện tại có thực thi cực tiểu hay cực đại hay không, đảm bảo rằng ở mỗi bước chỉ có các ứng cử viên chính xác vẫn còn cực trị. 

Khi cấu trúc phân cấp này được xây dựng, chúng tôi gán các giá trị hoán vị theo thứ tự tăng dần theo thứ tự DFS được xây dựng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute + điều chỉnh | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng DFS theo lớp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị bằng cách xây dựng cấu trúc nhị phân trên các chỉ mục mã hóa khi mỗi phần tử phải trở thành cực trị. 

1. Nhóm các chỉ số theo thời gian loại bỏ chúng. Các phần tử có a_i = -1 được coi là có thời gian L + 1, trong đó L là thời gian loại bỏ tối đa. Điều này xác định lớp sống sót cuối cùng. 
2. Sắp xếp các chỉ số theo thời gian loại bỏ giảm dần. Chúng tôi sẽ chỉ định cấu trúc từ trên cùng (người sống sót cuối cùng) xuống các phần tử bị loại bỏ sớm nhất. 
3. Duy trì hàm đệ quy gán một phân đoạn chỉ mục theo thứ tự hợp lệ phù hợp với ràng buộc lớp nhất định. Mỗi cuộc gọi tương ứng với một phân đoạn phải hoạt động như một “lớp tối thiểu” hoặc “lớp tối đa”. 
4. Đối với phân đoạn tương ứng với lớp tối thiểu, chúng tôi đảm bảo rằng phần tử trung tâm đã chọn (có thời gian tồn tại cao nhất trong phân đoạn đó) được đặt sao cho nó trở thành mức tối thiểu cục bộ tại lần lặp đó. Điều này buộc các hàng xóm phải lớn hơn theo thứ tự được xây dựng
