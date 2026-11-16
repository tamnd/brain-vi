---
title: "CF 1890F - Trò chơi xếp chồng"
description: "Mỗi ngăn xếp thuộc về một đỉnh. Nếu hiện tại chúng ta đang ở đỉnh u, chúng ta nhìn vào phần tử trên cùng của ngăn xếp u. Nếu ngăn xếp trống, quá trình sẽ dừng và trả về u. Nếu không, chúng ta bật phần tử trên cùng, nhảy đến đỉnh được ghi trên phần tử đó và lặp lại."
date: "2026-06-09T01:10:35+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1890
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 906 (Div. 2)"
rating: 3000
weight: 1890
solve_time_s: 50
verified: false
draft: false
---

[CF 1890F - Trò chơi xếp chồng](https://codeforces.com/problemset/problem/1890/F) 

**Đánh giá:** 3000 
**Thẻ:** dfs và tương tự, đồ thị, cách triển khai, cây 
**Thời gian giải:** 50s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ngăn xếp thuộc về một đỉnh. Nếu chúng ta hiện đang ở đỉnh`u`, chúng ta nhìn vào phần tử trên cùng của ngăn xếp`u`. 

Nếu ngăn xếp trống, quá trình dừng và trả về`u`. 

Nếu không, chúng ta bật phần tử trên cùng, nhảy đến đỉnh được ghi trên phần tử đó và lặp lại. 

Điều hấp dẫn là mọi cuộc gọi đều bắt đầu từ ngăn xếp ban đầu. Một bản nhạc pop được biểu diễn trong khi đánh giá`init(i)`không ảnh hưởng`init(j)`. 

Việc mô phỏng trực tiếp từ mọi đỉnh bắt đầu là vô vọng. Một lần thực thi có thể xuất hiện với tất cả các phần tử ngăn xếp và tổng số phần tử lớn bằng`10^6`. Chạy độc lập cho tất cả`n ≤ 10^5`đỉnh bắt đầu sẽ yêu cầu xung quanh`10^11`hoạt động trong trường hợp xấu nhất. 

Cấu trúc ẩn trong quy trình là chỉ có phần tử trên cùng hiện tại của mỗi ngăn xếp mới quan trọng. Tại bất kỳ thời điểm nào, mọi ngăn xếp không trống đều xác định chính xác một cạnh đi ra, từ chỉ mục của ngăn xếp đến giá trị cao nhất hiện tại của nó. Điều đó đưa ra một đồ thị chức năng. 

Một tình huống đặc biệt khó khăn là một chu kỳ có định hướng. 

Coi như:```
1: [2]
2: [1]
```Bắt đầu từ một trong hai đỉnh, chúng ta không lặp lại mãi mãi. Chuyến thăm đầu tiên tới đỉnh`1`tiêu thụ hàng đầu của nó. Chuyến thăm đầu tiên tới đỉnh`2`tiêu thụ hàng đầu của nó. Sau đó cả hai ngăn xếp đều trống nên quá trình cuối cùng sẽ kết thúc. 

Một DFS đơn giản trên đồ thị hàm số sẽ coi chu trình này là một vòng lặp vô hạn một cách không chính xác. Chu trình phải được giảm bớt bằng cách lấy ra một phần tử từ mỗi ngăn xếp trong chu trình. 

Một sai lầm dễ mắc phải khác là cho rằng một đỉnh chỉ có một đáp án. Cạnh đi của đỉnh thay đổi sau khi phần tử trên cùng của nó bị loại bỏ. Ví dụ:```
1: [2, 3]
2: []
3: []
```Đỉnh ngăn xếp đầu tiên`1`chỉ vào`3`, nhưng sau khi loại bỏ nó, điểm trên cùng tiếp theo sẽ là`2`. Bất kỳ giải pháp nào cố định vĩnh viễn một cạnh đi ra trên mỗi đỉnh sẽ làm mất thông tin cần thiết. 

Các ràng buộc gợi ý một giải pháp gần như tuyến tính. Tổng số phần tử ngăn xếp nhiều nhất là`10^6`, vì vậy chi tiêu`O(1)`hoặc`O(log n)`làm việc cho mỗi phần tử là tốt. Bất cứ điều gì cl
