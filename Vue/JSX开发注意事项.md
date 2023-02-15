## 针对slot传递参数

开发的组件
```js
{this.dataSource?.map(item => {
		return (
			<div class="item van-hairline--top" 
			key={item[this.valueKey]} 	
			on-click={() =>this.onSelect(item)}
			>
				<div class="title">
					{this.$scopedSlots.default(item)}
					{/*this.$scopedSlots.default({ item })*/}
				</div>
			</div>
		)
})}
```

调用组件
```html
<search-select-cmp
	v-model="params.userId"
	class="search-select"
	title="选择人员"
	:visible="personVisible"
	placeholder="请输入人员姓名"
	valueKey="userId"
	searchUrl="api/sysUser/v1/page"
	searchKey="userName"
	pageSizeKey="size"
	pageNumberKey="current"
	@close="onClosePersonSelect"
    @change="onSelectPerson"
  >
	<div slot-scope="item" class="title-item">{{item.userName}}</div>
</search-select-cmp>
```

## 针对$listeners和$attrs
组件
```js
  <van-popup
	  v-model={this.visible}
		{...{
			on:this.$listeners,
			attrs:this.$attrs
		}}
	>
	{/*或者直接 on={ this.$listeners }，不用在括号内 */}
		<div class="container">
			<div class="title van-hairline--bottom">{this.title}</div>
			<div class="content-container">{this.$slots.default}</div>
		</div>
	</van-popup>
```
组件调用
```js
<base-popup-cmp
	v-model={this.$attrs.visible}
	class="select-popup-container"
	attrs={this.$attrs}
	on-close={() =>this.$emit('close')}
	on-open={this.onOpen}
	style={{ height: '50%' }}
	>
```

## 属性调用
```js
<WrapTable >
  <c-table-column
    label="操作"
    width="180"
    scopedSlots={{
      default: scope => {
        return (
          <c-button>查看</c-button>
         )
      }
      }}></c-table-column>
</WrapTable>
```

