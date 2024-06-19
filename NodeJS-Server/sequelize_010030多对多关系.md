---
tags: sequelize node egg
---

# 多对多关系

## migration

**Work**

```js
'use strict';

module.exports = {
	async up (queryInterface, Sequelize) {
		const { INTEGER, DATE, STRING, TEXT, DOUBLE } = Sequelize;
		await queryInterface.createTable('works', {
			id: { type: INTEGER, primaryKey: true, autoIncrement: true, allowNull: false, },
			content: { type: TEXT, allowNull: false },
			status: { type: INTEGER, allowNull: false, defaultValue: 0 },
			path: { type: STRING, allowNull: true },
			coins: { type: DOUBLE, allowNull: false, defaultValue: 0 },
			userId: {
			type: INTEGER,
			field: 'user_id',
			reference: {
				model: {
					tableName: 'users'
				},
				key: "id"
			},
			allowNull: false
			},
			createdAt: { type: DATE, field: 'created_at', },
			updatedAt: { type: DATE, field: 'updated_at', },
		})
	},

	async down (queryInterface, Sequelize) {
		await queryInterface.dropTable('works');
	}
};
```

**Label**

```js
'use strict';

module.exports = {
	async up (queryInterface, Sequelize) {
		const { INTEGER, DATE, STRING, } = Sequelize;
		await queryInterface.createTable('labels', {
			id: { type: INTEGER, primaryKey: true, autoIncrement: true, allowNull: false, },
			name: { type: STRING(30), allowNull: false, unique: true, },
			createdAt: { type: DATE, field: 'created_at', },
			updatedAt: { type: DATE, field: 'updated_at', },
		})
	},
	
	async down (queryInterface, Sequelize) {
		await queryInterface.dropTable('labels');
	}
};
```

## model

